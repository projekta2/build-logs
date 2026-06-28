# 007 — BYOK architecture for AI Chrome extensions: zero backend cost

**Product:** PR Focus Pro · TabCost Pro
**Area:** Architecture / AI Integration
**Date:** 2026-06-29
**Status:** ✅ Shipped and in production
**Also published:** [dev.to — I built an AI Chrome extension with zero backend cost](https://dev.to/projekta2/SLUG_AQUI) ← actualiza con la URL real tras publicar

---

## The situation

When I decided to add AI features to PR Focus — diff summarization, risk scoring, one-click draft reviews — I had to answer one question before writing a single line of code:

**Where does the AI call actually happen?**

The answer to that question determines your privacy posture, your infrastructure cost, your support burden, and how you compete in a market where CodeRabbit, GitHub Copilot, and a dozen well-funded tools already exist.

I had three realistic options.

---

## The options on the table

### 1. Hosted backend — the standard SaaS play

A Node.js server holds a master API key. Users pay me. I pay the AI provider out of that margin.

**What it gives you:**
- Frictionless onboarding — users never see an API key
- Full model control
- Caching and rate limiting at the platform level

**What it costs:**
- Infrastructure to maintain as a solo developer: uptime, security, GDPR compliance, rate limiting under abuse
- PR diffs and code context pass through your server — a real trust barrier for private repos
- You're now a margin business: pay OpenAI $X, charge users $Y, keep the difference. That math gets tight fast against VC-funded competitors

### 2. Local model — bundling a small LLM

Ship a quantized model (Llama.cpp or similar) inside the extension itself.

**What it gives you:**
- Zero external dependencies after install
- No API costs, no trust issues, no network latency

**What it costs:**
- Extension size: ~2MB becomes 100MB+. Chrome Web Store has limits, and users notice the install size
- Model quality: even the best quantized model running locally in a browser context is nowhere near GPT-4o-mini or Llama 3.3 on Groq
- Low-RAM and mobile devices struggle

### 3. BYOK — Bring Your Own Key

Users create their own API key with whichever provider they choose and paste it into the extension settings.

**What it gives you:**
- The diff never touches your server. For a tool that reads private code, this answers the first question every developer asks
- Your AI bill: $0. The user pays their provider directly, at cost, with no markup
- No backend to maintain. One less system to wake up at 2am for

**What it costs:**
- Real onboarding friction: every user has to create an account with a provider and paste a key. Some bounce here
- Support complexity: wrong keys, expired keys, keys without the right model access — these become a support category

---

## What I chose, and why

BYOK. Three reasons outweighed everything else.

**Trust is load-bearing, not a feature.** The first question from every early user was "does my code go to your server?" With BYOK, the honest answer is no — and that one sentence has converted more users than any feature I've built. For a tool that reads GitHub diffs, trust isn't a differentiator. It's the threshold.

**Cost is predictable and fair.** Groq's free tier covers around 14,400 requests per day for smaller models. For an individual developer reviewing PRs, that's effectively free. For heavier use with OpenAI GPT-4o-mini: roughly $0.0001 per PR summary. A hundred PRs a day costs one cent. Users pay exactly what the AI costs — no markup, no monthly fee for a feature they might not use every day.

**Simplicity is survival.** As a solo developer, every hour on infrastructure is an hour not on the product. BYOK eliminates the server category entirely. No uptime monitoring. No security audit. No backend deployment. The extension is the product.

---

## The implementation

All four providers use the OpenAI-compatible `/v1/chat/completions` endpoint. One implementation, four providers:

```javascript
const AI_PROVIDERS = {
  groq: {
    endpoint: 'https://api.groq.com/openai/v1/chat/completions',
    model: 'llama-3.3-70b-versatile',
  },
  openai: {
    endpoint: 'https://api.openai.com/v1/chat/completions',
    model: 'gpt-4o-mini',
  },
  mistral: {
    endpoint: 'https://api.mistral.ai/v1/chat/completions',
    model: 'mistral-small-latest',
  },
  ollama: {
    endpoint: 'http://localhost:11434/v1/chat/completions',
    model: 'llama3.2', // fully local, zero API cost
  }
};

async function callAI(prompt) {
  const { aiApiKey, aiProvider } = await chrome.storage.local.get(['aiApiKey', 'aiProvider']);
  const config = AI_PROVIDERS[aiProvider] || AI_PROVIDERS.groq;

  const response = await fetch(config.endpoint, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${aiApiKey}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: config.model,
      messages: [{ role: 'user', content: prompt }],
      max_tokens: 500
    })
  });

  return response.json();
}
```

The key is stored in `chrome.storage.local`. It leaves the browser exactly once per call: directly to the provider. It never passes through any server I operate.

---

## What it cost

**Onboarding drop-off is real.** Some users see "paste your API key" and close the tab. I can see this in install-to-activation rates. The mitigation is leading with Groq (free tier, 2-minute setup) and making the core extension work without AI at all — sorting, export, multi-account support are all available without a key.

**Support is a real category.** The most common issues in order: key pasted with trailing whitespace, key without the right model tier, expired key, key from the wrong account. Specific error messages per status code eliminate most of these:

```javascript
if (response.status === 401) return 'Invalid key — check you copied it completely.';
if (response.status === 429) return 'Rate limit — you\'ve hit the free tier ceiling.';
if (response.status === 403) return 'Permission denied — this key may not access this model.';
```

**Model deprecations require a release.** When Groq deprecated an older model, users on that version got errors until I pushed an update. The fix: store the default model name in the provider config, not hardcoded in the fetch call, so a config update is enough.

---

## Would I do it again?

Yes. And I'd choose it earlier.

When I first posted about PR Focus on Reddit, the top comment was "does this send my code to your server?" — and the answer being no was the deciding factor for multiple installs. That dynamic hasn't changed.

If PR Focus were a hosted SaaS, I'd be competing on price and features against tools with real engineering teams and VC budgets. BYOK lets me compete on trust and transparency instead. That's a fight I can win.

The right question isn't "BYOK or hosted?" — it's "who is your user?" If your users are developers who understand what an API key is and care about where their code goes, BYOK is the correct architecture. If your users are non-technical and "API key" is a foreign concept, BYOK will kill your conversion rate before you can demonstrate any value.

For a tool built for developers who review code on GitHub: BYOK was the only architecture that made sense.

---

*This decision connects directly to [Build Log #001](./001-pr-focus-risk-scoring.md) — the hybrid risk scoring architecture also relies on BYOK to avoid sending diffs to a backend at risk-scoring time.*

*Prompted in part by multiple questions in the dev.to comments asking how the AI integration works without a subscription fee. The short answer was always BYOK. This log is the long answer.*
