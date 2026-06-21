# Why PR Focus is BYOK instead of a hosted AI backend

**Product:** PR Focus  
**Date:** 2026-06-21  
**Status:** Shipped  

---

## The situation

When I started building PR Focus, I knew I needed AI to summarize diffs, score risk, and draft reviews. The question wasn't *if* — it was *how* to deliver it without building a backend I couldn't afford to maintain.

I weighed three paths:

1. **Hosted backend (SaaS)** — Node.js server, master API key, subscription billing, eat the AI cost.
2. **Local model** — bundle a small LLM with the extension.
3. **BYOK** — users bring their own OpenAI/Groq/Mistral key.

I chose BYOK. Here's why — and what it cost me.

---

## The options on the table

### 1. Hosted backend — the "standard" SaaS play

This is what most products do. Build a server, hold a master API key, charge users monthly, and pay the AI provider out of that revenue.

**Pros:**
- Users never see an API key — frictionless onboarding.
- Full control over which models are used.
- Can cache responses to reduce costs.

**Cons:**
- I'm a solo dev. Servers mean security audits, uptime monitoring, GDPR compliance, and handling rate limits at scale.
- I'd have to mark up AI costs 2–3x just to stay profitable — and I hate that.
- Every PR diff would pass through my infra. Trust barrier for private repos.

### 2. Local model — the "pure" approach

Ship a quantized LLM (like Llama.cpp) with the extension.

**Pros:**
- No external dependencies. No API costs. Zero trust issues.

**Cons:**
- Extension size would balloon from ~2MB to >100MB. Chrome Web Store has limits.
- Mobile devices and low-RAM machines would struggle.
- Quality would be far below GPT-4o or Groq.

### 3. BYOK — the "radical" choice

Users create their own API key with their provider of choice and paste it into the extension.

**Pros:**
- 🔒 Trust: PR diffs never touch my server. Huge for developers reviewing private repos.
- 💰 Cost: Groq's free tier covers 95% of users. No markup needed.
- 🧘 Simplicity: No backend to maintain as a solo dev. Just the extension.

**Cons:**
- 😓 User friction: Every user has to create an account and paste a key. Some bounce.
- 🧩 Support complexity: Users paste wrong keys, or keys without permissions, and open issues saying "AI isn't working."

---

## What I chose, and why

**I chose BYOK.** Three reasons outweighed everything else:

1. **Trust is non-negotiable.** The first question developers ask is "is my code safe?" With BYOK, I can honestly say: "it never leaves your browser." That single sentence has closed more sales than any feature I've built.

2. **Cost is predictable.** Groq's free tier handles ~95% of individual workflows. For the rest, users pay exactly what the AI costs — no markup, no subscription.

3. **Simplicity is survival.** As a solo developer, every hour spent on server maintenance is an hour not spent on the product. BYOK lets me focus on the extension.

## What it cost

The biggest cost is **user friction.** Every user has to:
1. Create an account with Groq/OpenAI (a few minutes).
2. Generate an API key.
3. Paste it into the extension.

Some users bounce at this step. I lose them.

The second cost is **support complexity.** Users paste the wrong key, or they paste a key without enough permissions, and they open an issue saying "AI isn't working." It's manageable, but it's a real support category.

## Would I do it again?

Yes. In fact, it's become my strongest selling point.

When I first posted about PR Focus on Reddit, the top question was "is this sending my code to your server?" — and the answer "no, BYOK" was the deciding factor for several users.

If I had built a hosted SaaS, I'd be competing directly with CodeRabbit, Copilot, and every well-funded AI review tool. BYOK lets me compete on **trust and transparency** instead of features or price.

---

*Have a different take on BYOK vs. SaaS? Open a discussion in [Issues](../../issues).*
