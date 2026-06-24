# 003 — Why Gumroad instead of Stripe, Paddle, or Lemon Squeezy

**Product:** TabCost Pro, PR Focus Pro, ChainTrace  
**Decision Area:** Payments stack / Business model  
**Status:** ✅ In production

---

## The context

When a product has to charge money, the first real decision is: who processes the payment?

In 2024–2026, the answer for an indie maker building Chrome extensions isn't obvious. There are four serious options: Stripe, Paddle, Lemon Squeezy, and Gumroad. Each has vocal fans. I chose Gumroad. Here's the reasoning.

---

## The options on the table

### Stripe

The industry standard. Any developer who has integrated payments once knows Stripe.

**Pros:**
- Full control over the purchase flow
- Powerful and well-documented webhooks
- Support for almost any use case

**Why I didn't choose it:**
- Stripe is not a Merchant of Record (MoR). I would be responsible for managing IVA, VAT, country-specific taxes, and tax compliance in every market where I sell. I'm a one-person indie maker building Chrome extensions; I don't have the capacity to manage that.
- The integration requires a backend for the payment confirmation webhook. BYOK removes my AI backend, but Stripe forces it back for licensing.
- Setup complexity is disproportionate for $5–$49 products.

### Paddle

Paddle *is* an MoR. It handles all taxes. It has a good reputation.

**Pros:**
- Full MoR
- Very well-thought-out subscription flows
- Customizable checkout overlay

**Why I didn't choose it:**
- At certain times, it required a minimum of $50/month in revenue to access their basic tier (pricing structures have changed, but the perception that "Paddle is for when you already have traction" persists)
- The onboarding process in 2024 was slower and required manual review for some products
- Better suited for SaaS with subscriptions, not one-time payments for Chrome extensions

### Lemon Squeezy

MoR, modern design, strong indie community. In 2024 it was the "cool" alternative to Paddle.

**Pros:**
- Full MoR
- Very clean checkout UI
- Built-in for software licensing

**Why I didn't choose it:**
- At the time I was making this decision, Lemon Squeezy was acquired by Stripe. The uncertainty about the product's direction post-acquisition was real.
- The Chrome extension ecosystem has fewer documented integration examples than Gumroad.

### Gumroad

MoR. Designed for creators. No monthly setup fee. Transaction-based commission (10% flat on the free tier, less if you pay monthly).

**Pros:**
- Full MoR: Gumroad handles European VAT, Australian GST, US sales tax. I don't touch any of it.
- Built-in software licensing: generates unique keys, allows you to configure the number of uses, and has an API to validate keys from the extension.
- No fixed monthly fee: if I sell nothing in a month, I pay $0.
- Setup in 20 minutes. Product page, price, license, done.
- The gumroad.com URL is recognizable to indie buyers. Reduces trust friction.

**Real drawbacks:**
- Checkout is not natively embeddable on the landing page (though there is an overlay with JS)
- The purchase URL is gumroad.com/l/xxx, not your domain
- 10% commission is high for low-margin products. At $5 (TabCost), Gumroad takes $0.50 plus Stripe fees. The real margin per TabCost sale is ~$3.80.
- Gumroad has had controversies regarding its product direction and communication with creators in the past.

---

## Why I chose Gumroad anyway

**The deciding factor was the MoR.**

I sell to developers worldwide. A user in Germany buying TabCost Pro generates European VAT. One in Australia generates GST. If I use Stripe, I am responsible for calculating, collecting, filing, and paying those taxes in every relevant jurisdiction. That is not viable as a one-person operation.

Gumroad takes on that full responsibility. I pay the 10% commission and in exchange, I don't have to hire an international tax advisor.

**The second factor was speed.**

With Stripe, I need a backend for license webhooks. With Gumroad, license validation is done with a call to their public API directly from the extension. In practice, integrating Gumroad into a Chrome extension is substantially simpler.

**The third factor was alignment of incentives.**

Gumroad is designed for creator products with one-time payments. They aren't doing me a favor by supporting that model; it's their core business. With Paddle or Stripe, one-time payments are the least optimized use case.

---

## What I would do differently

**Gumroad's checkout overlay on landing pages could be better.** The embed JS exists but isn't as polished as Lemon Squeezy's embeddable checkout. If this visibly affects conversion (something I plan to measure), I would consider Lemon Squeezy for a future product once the Stripe post-acquisition situation settles down.

**The 10% commission hurts on low-priced products.** For TabCost ($5), it's almost philosophical. For PR Focus ($9.50), it's tolerable. If ChainTrace ($49) scales in volume, it makes sense to migrate to Gumroad's paid tier ($10/month flat instead of 10%) when monthly revenue exceeds $100.

---

## Would I do it again?

For the current stack of Chrome extensions with one-time payments: yes. The simplicity of MoR + integrated licensing + zero fixed monthly fee is the right combination for this stage.

---

**Next log:** [004 — The migration to Manifest V3: what broke and what we gained](./004-mv3-migration.md)

**Tags:** `payments` `gumroad` `business-model` `licensing` `indie-maker` `merchant-of-record`
