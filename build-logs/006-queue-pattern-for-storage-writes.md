# 006 — Queue Pattern for `chrome.storage.local` Writes in MV3

**Product:** TabCost Pro  
**Area:** Concurrency / Manifest V3  
**Status:** ✅ Shipped and in production

---

## The situation

In TabCost Pro, I need to persist the accumulated idle time for each browser tab. Multiple events can trigger updates to the same `tabTimers` object:

- `chrome.tabs.onActivated` — when the user switches to a different tab
- `chrome.tabs.onUpdated` — when a tab's URL or status changes
- `chrome.alarms.onAlarm` — periodic tick (every minute) to flush accumulated time
- `chrome.tabs.onRemoved` — when a tab is closed

In Manifest V2, this was straightforward: I held the `tabTimers` object in memory in the background script, and mutated it directly. All events shared the same in-memory object, so there was no risk of concurrent writes clobbering each other.

In Manifest V3, the Service Worker can be terminated between events. Any state that needs to survive must be written to `chrome.storage.local` — which is persistent but async, and crucially, not atomic across multiple writers.

## The options on the table

I considered several approaches:

### 1. Write directly from each event (naive approach)

```javascript
// ❌ This is what the simplified example showed — it has a race condition
async function updateTimer(tabId, elapsed) {
  const { tabTimers = {} } = await chrome.storage.local.get('tabTimers');
  tabTimers[tabId] = (tabTimers[tabId] || 0) + elapsed;
  await chrome.storage.local.set({ tabTimers });
}
```

The problem: Two events can both read the same `tabTimers` object, modify their own slice, and write back. The second write will overwrite the first, losing data. In MV3, this is more likely because Service Workers can be woken up by different events concurrently, and `chrome.storage.local` operations are async, leaving a window for interleaving.

Why it's tempting: Simple, short, looks like it works in local testing. It's the kind of code that passes a quick smoke test and fails under real concurrency.

### 2. Use `chrome.storage.local` with a mutex-like lock

I explored using a simple lock with `chrome.storage.local` to guard writes:

```javascript
// ❌ Pseudocode — tricky to implement correctly across async events
async function withLock(key, fn) {
  // Try to acquire lock, retry on conflict...
  // But lock acquisition itself is a race condition in MV3.
}
```

Why it's not viable: Building a correct lock with `chrome.storage.local` is hard. The lock acquisition is itself a read-modify-write operation that suffers from the same race condition. Retry loops can lead to livelocks or excessive storage operations. And because the Service Worker can be terminated at any time, you can't rely on holding a lock across multiple `await` boundaries.

### 3. Single-writer queue (the chosen approach)

```javascript
// ✅ This is what I actually shipped
class TimerStore {
  constructor() {
    this.queue = [];
    this.processing = false;
  }

  async update(tabId, elapsed) {
    this.queue.push({ tabId, elapsed });
    if (!this.processing) {
      this.processing = true;
      // We intentionally don't await here to prevent concurrent processing.
      // Instead, processQueue() runs asynchronously and handles all queued items.
      this.processQueue().finally(() => {
        this.processing = false;
      });
    }
  }

  async processQueue() {
    while (this.queue.length > 0) {
      // Take all currently queued items atomically
      const updates = this.queue.splice(0);
      if (updates.length === 0) continue;

      // Read once, apply all updates, write once
      const { tabTimers = {} } = await chrome.storage.local.get('tabTimers');
      for (const { tabId, elapsed } of updates) {
        if (typeof elapsed === 'number') {
          tabTimers[tabId] = (tabTimers[tabId] || 0) + elapsed;
        }
      }
      await chrome.storage.local.set({ tabTimers });
    }
  }
}
```

How it works: All updates are pushed into a queue. The queue is processed by a single "writer" that reads the current state, applies all pending updates, and writes back. Because updates are applied in batches, the number of storage operations is reduced, and the race condition is eliminated.

The key insight: We accept that multiple events can enqueue updates concurrently — that's fine, because the queue itself is just an in-memory array that all events share. The only critical section is the read-modify-write cycle, and we ensure that only one such cycle runs at a time by using the `processing` flag.

Edge cases:

- If the Service Worker dies while `processQueue()` is running, any unprocessed updates are lost. I accept this trade-off because the cost counter is not critical data — it's a UI convenience. For more important state, you'd want to flush the queue to storage more aggressively or use a more durable approach.

- To mitigate this, I also flush the queue on `chrome.runtime.onSuspend` (which is called when the Service Worker is about to be terminated), though this is not 100% reliable in MV3.

## What I chose, and why

I chose the single‑writer queue pattern because it's simple, robust, and has minimal overhead. It solves the race condition without introducing locks or complex retry logic. The code is easy to reason about and test.

This pattern is not unique to Chrome extensions — it's a common concurrency control pattern in many systems. The twist in MV3 is that the writer must be able to survive Service Worker terminations, which is why I keep the queue in memory and rely on `onSuspend` to flush it.

## What it cost

Simplicity traded for a small risk of data loss on Service Worker termination. In practice, this has not been a problem for TabCost Pro because the timer data is recalculated from the actual tab activity when the extension restarts. The queue is a performance optimisation and a concurrency control measure, not the source of truth.

The queue adds a small delay between an event happening and the storage being updated. In practice, this is measured in milliseconds, and the UI is updated separately (via the popup's own timer), so users don't notice.

## Would I do it again?

Yes. The single‑writer queue pattern is a solid solution for this class of problem. It's been running in production for TabCost Pro without any concurrency-related bugs.

For future projects, I'd consider this pattern as the default for any state that is written by multiple concurrent events in an MV3 Service Worker.

---

*This log was prompted by a sharp observation from Nazar Boyko on [this dev.to article](https://dev.to/projekta2/migrating-to-manifest-v3-what-actually-broke-what-we-saved-and-what-we-gained-ed1), who pointed out the race condition in the simplified example. The code above is what I actually shipped after that discussion.*
