# Contributing to Build Logs

Thanks for being here. This document covers both ways to participate: posting a Build Log and joining a Review Swap thread.

---

## Review Swap — full rules

### Before you post

1. Check the [pinned thread](../../issues?q=is%3Aissue+is%3Aopen+label%3Areview-swap) for the current month. There is always exactly one open at a time.
2. Make sure your PR is **public** — a personal project, an open source contribution, anything anyone can actually open and read.
3. Make sure it's still **open** (not merged, not closed) — reviewers need to be able to leave comments that matter.

### Posting your PR

Comment on the open thread using this format:

```markdown
**PR:** <link to your pull request>
**Repo context:** one sentence — what does this project do
**What I want feedback on:** be specific (e.g. "error handling in the retry logic", "is this the right abstraction", "naming")
**Owe a review to:** _(leave blank until you've picked one)_
```

### Reviewing someone else's PR

1. Scroll the thread and find a PR that doesn't have a reviewer assigned yet.
2. Reply to your own original comment, editing the **Owe a review to** line with a link to the PR you're reviewing.
3. Go leave the actual review **on their PR**, not in this thread — this thread is for coordination, the real feedback belongs on their pull request where it's actionable.
4. Drop one line back in this thread once done: `Reviewed → <link>`.

### What counts as a real review

A review needs **at least one** of the following to count:

- A comment on a specific line explaining a concrete risk, bug, or edge case — not a vague "this could be cleaner"
- A genuine question about a decision the author may not have considered
- Explicit recognition of something done well, with a reason why

A review does **not** count if it is:

- "LGTM" or equivalent with no further comment
- An emoji-only reaction
- A review of your own PR
- Copy-pasted across multiple PRs in the same thread

Reviews that don't count will be flagged by a maintainer comment in the thread. First time, it's a friendly note. Repeated low-effort "reviews" used to inflate your swap count will result in losing posting access to future threads — this is the only enforcement mechanism here, and it exists to keep the swap fair for everyone who puts in real effort.

### Closing the thread

At the end of the month, the thread is closed and archived to [`review-swap/archive/`](./review-swap/archive/). The [Reviewers Board](./README.md#-reviewers-board) in the main README is updated from the archive. A new thread opens within the first few days of the following month using [`review-swap-template.md`](./.github/ISSUE_TEMPLATE/review-swap-template.md).

---

## Build Logs — how to suggest or contribute one

Build Logs are currently written by the repo maintainer, based on real decisions from shipping [TabCost](https://github.com/projekta2/tabcost-pro-source), [PR Focus](https://github.com/projekta2/pr-focus-landing), and ChainTrace.

**Guest logs are welcome** if you're a solo or small-team dev shipping something real and want to write about an actual engineering decision — not a tutorial, not a "10 tips" post. To propose one:

1. Open an issue with the label `build-log-proposal`
2. Include: what you built, the specific decision or trade-off you want to write about, and a draft outline (3-5 bullet points)
3. If it fits the spirit of this repo (real decisions, real trade-offs, no fluff), you'll get a go-ahead and a slot in [`build-logs/`](./build-logs/)

Format reference: see [`build-logs/_TEMPLATE.md`](./build-logs/_TEMPLATE.md) for structure.

---

## General etiquette

This repo runs on the same principle as the Review Swap itself: show up, contribute something real, and don't waste other people's time. See [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) for the formal version.

Questions? Open an issue with the label `question`, or email hello@projekta2.com.
