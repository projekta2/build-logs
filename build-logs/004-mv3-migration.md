#004 — Migrating to Manifest V3: What We Broke, What We Saved, What We Gained

**Product:** TabCost Pro, PR Focus Pro, ChainTrace
**Decision Area:** Architecture / Compatibility
**Status:** ✅ All three extensions are MV3-native from the start

---

## The Context

If you're building Chrome extensions in 2024-2026, Manifest V3 isn't optional. Chrome has removed MV2 extensions from the Web Store. There's no debate.

But when I started building, the MV3 documentation was gaps, the community was in the middle of transitioning, and many of the available tutorials taught MV2 patterns that no longer work.

Making the decision to build MV3-native from scratch—instead of porting from MV2—was the project's most significant architectural decision. Here's what it entailed.

---

## What Really Changed in MV3

For those who haven't experienced the transition, here's a brief summary:

**Service Workers instead of persistent Background Pages.** In MV2, you had a `background.js` file that lived in memory all the time. In MV3, you have a Service Worker that Chrome can terminate at any time when it's not active, and it restarts on demand.

The real consequence: you can't save state in global variables of the background script. If the Service Worker dies while processing something, that state disappears. Any data that needs to survive must go to `chrome.storage`.

**No `webRequestBlocking` for regular extensions.** MV2 allowed blocking and modifying HTTP requests. MV3 removes this for non-enterprise extensions. This massively affects ad blockers. It wasn't a problem for my extensions, but I had to verify it.

**Declarative Net Request instead of programmatic.** Related to the above. Network rules are now declarative and static. More secure, less flexible.

**Restrictions on eval() and dynamic code.** MV3 prohibits executing dynamic code from remote strings. If your extension downloaded and executed a script, that's dead. This affects some remote configuration architectures.

---

## The specific decisions I made

### 1. Chrome Storage as the sole source of truth

In TabCost Pro, the cost-per-tab ticker must persist even if the Service Worker restarts. The solution: all relevant state (user hourly rate, time accumulated per tab ID, configuration) resides in `chrome.storage.local`. The Service Worker reads it at startup and writes it with each significant change.

``javascript
// ❌ MV2 thinking — this is lost when the Service Worker dies
let tabTimers = {};

// ✅ MV3 reality — persisting in storage
async function updateTabTimer(tabId, elapsed) {

const data = await chrome.storage.local.get('tabTimers');

const timers = data.tabTimers || {};

timers[tabId] = (timers[tabId] || 0) + elapsed;

await chrome.storage.local.set({ tabTimers: timers });

}
```

The cost: more writes to storage. Chrome storage has limits (5MB in `local`, 100KB in `sync`). With many tabs open for hours, it's manageable. I monitored it during development.

### 2. IndexedDB for large datasets in PR Focus

PR Focus stores PR summaries, risk scores, and metadata per PR. With large repositories (100+ PRs), this easily exceeds the limits of `chrome.storage.local`. The solution: IndexedDB accessible from the popup and from content scripts, with the Service Worker acting as the coordinator.

The complication: IndexedDB isn't directly accessible from Service Workers in all versions of Chrome (this changed in Chrome 102, but you need to check the minimum supported version). I had to verify support and add a fallback.

### 3. Alarm API for Recurring Tasks

In MV2, you could use `setInterval` in the background script for recurring tasks. In MV3 with Service Workers, a `setInterval` that's running when Chrome kills the Service Worker simply... stops.


The solution is `chrome.alarms`:

```javascript
// Register alarm upon extension installation
chrome.runtime.onInstalled.addListener(() => {
chrome.alarms.create('tabCostTick', { periodInMinutes: 1 });
});

/ Respond to the alarm
chrome.alarms.onAlarm.addListener((alarm) => {
if (alarm.name === 'tabCostTick') {
updateAllTabCosts();

}
});
``

Limitation: The minimum alarm frequency in MV3 is 1 minute. For TabCost, which needs to update the cost in near real-time, this was a problem. The solution: The popup has its own internal timer when open, and the 1-minute alarm persists changes to storage when the popup is closed.

### 4. Stricter Content Security Policy

MV3 requires a more restrictive CSP. No `unsafe-eval`, no inline scripts in the extension's HTML. For PR Focus, which uses dynamically generated review templates, I had to move all HTML generation to pure JavaScript (createElement, textContent) instead of innerHTML with strings.

```javascript
// ❌ Blocked in MV3 if the string comes from a dynamic source
popup.innerHTML = `<div class=
