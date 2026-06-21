# Why PR risk scoring is a hybrid, not a pure AI verdict

**Product:** PR Focus
**Date:** 2026-06-21
**Status:** Shipped

---

## The situation

PR Focus exists because reviewing pull requests in the order they arrive is a bad default. A one-line typo fix and a migration touching the auth flow both show up in your inbox the same way: a row in a list. The obvious first version of "smart sorting" is just sorting by age — oldest PR first. I shipped that in an early prototype and it was immediately wrong in practice: a two-day-old PR that touches database schema is not less urgent than a two-week-old PR that fixes a comment typo.

The real question wasn't "what order should PRs appear in" — it was "how do you cheaply approximate how much attention a PR actually deserves, without making the reviewer read the diff first to find out."

## The options on the table

1. **Sort by age only** — simplest possible thing. Cheap, predictable, and wrong often enough that people would stop trusting the sort and go back to manual triage, which defeats the point of the extension.

2. **Sort by CI status only** — failing CI bumped to top. Better signal than age alone, but it misses the most dangerous category entirely: PRs that pass CI cleanly but touch something structurally risky (auth, billing, database migrations) where tests often don't cover the actual risk.

3. **Full static analysis** — actually parse the diff, build a dependency graph, flag changes to sensitive files using AST-level analysis. Most accurate in theory. Completely impractical for a Chrome extension meant to be lightweight and BYOK — this would mean shipping a real analysis engine, not a popup tool, and would blow past what a freemium extension should cost to run.

4. **AI risk score from the diff** — send the diff (or a summarized version) to whatever LLM provider the user has configured, ask for a 0–100 risk estimate with a short reason, and combine that with CI status and age into one sort.

## What I chose, and why

Option 4, but not as a 100%-AI-only score — a **hybrid**: CI failures and PR age are deterministic inputs computed locally for free, and the AI risk score is an additional weighted signal layered on top, not a replacement for the cheap heuristics.

The reasoning: pure AI scoring sounds more impressive, but it's slower, costs the user tokens on every poll, and — this matters more than it sounds — an LLM's risk read can be wrong or inconsistent between two calls on the same diff. A CI failure is never ambiguous. PR age is never ambiguous. Building the priority queue so the deterministic signals always have a floor effect (a failing CI PR never sorts below a passing one, regardless of what the AI says) meant the worst case for a wrong AI score is "imperfect ranking," not "actively misleading ranking."

This is also why PR Focus is BYOK instead of running a hosted model: a risk score that costs the user real API spend on every refresh needed to be something they could see the actual cost of, using a key they control, not a black box subscription fee.

## What it cost

The hybrid approach is harder to explain in one line than "AI does the sorting" — and harder to market, frankly. It also means the AI piece is doing genuinely less work than a pure-AI pitch would suggest: it's a tiebreaker and an amplifier on top of two boring signals, not the whole system. Early feedback from one tester was "so is it really AI-powered or not" — a fair question, and the honest answer is: yes, but it's deliberately not load-bearing on its own.

The other real cost is that the risk reasons the AI returns are only as good as the diff context it's given, and for very large PRs, summarizing the diff before sending it to keep token costs sane means the model is reasoning over a lossy compression of the actual change. That's a real limitation, not a hypothetical one — there's no version of this design where it disappears completely.

## Would I do it again?

Yes, for a tool meant to run as a lightweight, BYOK browser extension. If PR Focus were a hosted service with its own backend and predictable infra spend, I'd push further toward option 3 — real static analysis would catch structural risk that no LLM read of a diff reliably will. But for what this is, the hybrid was the right trade: deterministic floor, AI ceiling, user pays for exactly the AI calls they make.

---

*Have a different take, or run into a similar trade-off building review tooling? Open a discussion in [Issues](../../issues).*
