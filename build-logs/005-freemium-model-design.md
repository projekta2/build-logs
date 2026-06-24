# 005 — Freemium without dark patterns: how I designed the free tier limits

**Product:** PR Focus Pro, ChainTrace  
**Decision Area:** Business model / UX  
**Status:** ✅ In production

---

## The context

Freemium is easy to get wrong. Most modern freemium products use the free tier as a hook: they give you just enough to make you dependent, then block the feature you need at the worst possible moment to force conversion. It's a dark pattern dressed up as a business strategy.

When I designed the tiers for PR Focus Pro and ChainTrace, I wanted to do something different. Here's the reasoning behind each limit.

---

## The principle that guided the design

**The free tier has to be genuinely useful, not deliberately frustrating.**

This is easier said than done, because it means resisting the temptation to turn the free tier into a camouflaged demo. A user who uses the free tier for 3 months and feels it delivers real value is a user who will eventually recommend the product, even if they never pay. That user is worth more in the long run than one who converts to PRO in the first week because you frustrated them enough.

---

## PR Focus Pro — Free tier design

### The options I considered

**Option A: PRs per day limit**  
*5 PRs with AI summary per day free, unlimited on PRO.*  
Problem: the number of PRs someone has to review varies wildly day by day. One day you might have 1, another you might have 20. A daily limit will hit you at the worst possible moment (when you need it most), which is exactly what I want to avoid.

**Option B: Repository limit**  
*1 repository on free, unlimited on PRO.*  
Problem: most individual developers work on one repo at a time. This limit doesn't frustrate the typical user, but it also doesn't meaningfully differentiate PRO for them.

**Option C: Full features on free, advanced features on PRO**  
*AI summaries free, risk scoring + draft review generation on PRO.*  
This was the direction I went with.

### What's on the free tier (and why)

- **AI summaries per PR:** The product's core value proposition. If the summary isn't free, there's no way for the user to understand what they're buying. Leaving it free isn't generosity; it's removing evaluation friction.
- **Basic risk score (0-100):** The score calculated with non-AI heuristics (diff size, files touched, CI status, age) is on the free tier. The user sees the number and understands it has value.
- **Multi-account support:** Because a developer might have a personal account plus a work account. Restricting this would penalize a legitimate use case.

### What's on PRO ($9.50 one-time)

- **Hybrid AI risk score:** The score that combines heuristics + AI semantic analysis. More accurate, especially on PRs with complex logic changes.
- **One-click draft review generation:** The most differentiating feature. Generates a draft review with key points, ready to edit. This is what justifies the price.
- **Priority support:** Not a technical feature, but it has real value for teams.

### Why the limit is here and not elsewhere

Draft review generation is a feature that requires an AI call per PR. With BYOK, the cost is the user's, not mine. But it makes sense as a dividing line because:
1. It's clearly differentiating (you can't simulate its value with the free tier)
2. The user who wants it has already experienced the summaries and scoring, and knows the product works
3. $9.50 one-time for something that saves you 30+ minutes per week pays for itself in the first week

---

## ChainTrace — Free tier design

ChainTrace has a different problem: the product's value is directly proportional to the volume of data you process. A feature-based limit doesn't make sense here; the natural limit is by volume.

### The options

**Option A: Rows per export limit**  
*100 rows free, unlimited on Premium.*  
Too low. A small Amazon Business catalog already has 200 products. The user would get frustrated on their first real use.

**Option B: Exports per month limit**  
*10 exports/month free, unlimited on Premium.*  
Problem: "export" is hard to define. One button click? A full session with pagination? The friction of counting it is technical, and the user perceives it as confusing.

**Option C: Captures per month limit + rows per capture limit**  
*50 captures/month, 500 rows per capture on free. Unlimited on Premium.*  
This is the one I chose.

### Why these numbers

**50 captures/month:** A user evaluating the product in a single day can do 10-15 captures without issue. A user who uses it regularly in their workflow needs more. The 50 limit leaves room for real evaluation (a week of normal use) and starts to pinch for intensive professional use. That's what I want: for the heavy user to notice it, not for the evaluator to trip over it.

**500 rows per capture:** Enough for medium-sized catalogs. Insufficient for procurement at scale. Again: the user doing professional procurement (the one paying $49) has needs that the free tier simply can't cover without making it uncomfortable.

---

## The anti-pattern I rejected: the "upgrade modal"

The most common freemium tactic is the upgrade modal: the user clicks on something in PRO, a big modal pops up saying "UPGRADE NOW to unlock this feature!", with a bright green button and a fake urgency counter.

I didn't implement it. In PR Focus Pro, PRO features have a discreet badge ("PRO") and hovering shows a tooltip: *"Upgrade to PRO — $9.50 one-time, no subscription."* If the user clicks, they go straight to the Gumroad page. No modal, no countdown, no "LIMITED TIME OFFER".

Does it convert less than an aggressive modal? Probably. It's a trade-off I consciously accept because the experience of the user who doesn't upgrade should not be degraded to pressure the one who will.

---

## What I learned that I didn't expect

**The free tier generates more referral traffic than the paid one.**  
PRO users rarely talk about the product publicly because they've already gotten what they wanted. Free tier users who find value in it are the ones writing on Reddit, in team Slacks, in Discord groups. The free tier is the most efficient distribution channel.

**The price matters less than the model.**  
Several users who bought PRO explicitly mentioned the one-time payment as the reason for purchasing. Not the specific feature, not the exact price. The model. "I don't want another subscription" is an objection that $9.50 one-time eliminates at the root.

---

## Would I do it again?

The tier design: yes, with the same principles.

What I would add: more detailed conversion metrics per touchpoint. I now know how many free users convert to PRO globally, but I don't know at which point in the flow the user decides to purchase. That would come in a v2 of instrumentation.

---

**Tags:** `freemium` `pricing` `ux` `business-model` `dark-patterns` `product-design` `chrome-extension`
