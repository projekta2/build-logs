<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1e3a5f,100:0ea5e9&height=220&section=header&text=Build%20Logs&fontSize=70&fontColor=ffffff&animation=fadeIn&fontAlignY=36&desc=Real%20engineering%20decisions%20from%20indie%20products%20in%20production&descAlignY=58&descSize=16&descColor=cccccc" width="100%" />
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=500&size=18&pause=1400&color=0EA5E9&center=true&vCenter=true&width=720&lines=No+theory.+Just+decisions+that+shipped.;Real+code%2C+real+tradeoffs%2C+real+mistakes.;Get+your+PR+reviewed+by+a+real+person+%E2%86%92+review+one+back." alt="Typing SVG" />
</p>

<p align="center">
  <a href="#-this-months-review-swap"><img src="https://img.shields.io/badge/Review_Swap-Open_now-0ea5e9?style=for-the-badge&labelColor=0f172a" /></a>
  <a href="#-build-logs"><img src="https://img.shields.io/badge/Build_Logs-Browse-1e3a5f?style=for-the-badge&labelColor=0f172a" /></a>
  <a href="#-reviewers-board"><img src="https://img.shields.io/badge/Reviewers-Join_the_board-22c55e?style=for-the-badge&labelColor=0f172a" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/projekta2/build-logs?style=flat-square&color=0ea5e9&labelColor=0f172a" />
  <img src="https://img.shields.io/github/license/projekta2/build-logs?style=flat-square&color=64748b&labelColor=0f172a" />
  <img src="https://img.shields.io/badge/Maintained_by-Alexandre_Iglesias-1e3a5f?style=flat-square&labelColor=0f172a" />
  <img src="https://img.shields.io/badge/Updates-As_decisions_happen-22c55e?style=flat-square&labelColor=0f172a" />
  <img src="https://img.shields.io/github/last-commit/projekta2/build-logs?style=flat-square&color=22c55e&labelColor=0f172a&label=Last%20update" />
</p>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## What this is

<br />

> Most "awesome" repos are link dumps. Most dev blogs are SEO bait. This is neither.

**Build Logs** is a public record of real engineering decisions made while building three production Chrome extensions — [TabCost](https://github.com/projekta2/tabcost-pro-source), [PR Focus](https://github.com/projekta2/pr-focus-landing), and [ChainTrace](https://github.com/projekta2/chaintrace-landing) — written by one indie developer, self-funded, no VC, no team.

Each log breaks down **one specific decision**: the realistic alternatives that were on the table, what was chosen, what it cost, and an honest answer on whether it'd be chosen again. No filler, no "10 tips" listicles. If a decision turned out to be wrong, the log says so.

Alongside the logs, this repo runs something most repos don't: a **monthly Review Swap** — a place where you post a real PR you want feedback on, and you review someone else's in return. No audience required to get value. No vote-and-wait mechanic. You get a real review, today, from a real person.

<br />

<table width="100%">
  <tr>
    <td width="50%" valign="top" align="center">
      <br />
      <h3>📓 Build Logs</h3>
      <p><sub>Read how real architecture decisions were made — and which ones I'd undo.</sub></p>
      <a href="#-build-logs"><strong>Browse the logs →</strong></a>
      <br /><br />
    </td>
    <td width="50%" valign="top" align="center">
      <br />
      <h3>🔁 Review Swap</h3>
      <p><sub>Post your PR. Review someone else's. Get real feedback, not a like.</sub></p>
      <a href="#-this-months-review-swap"><strong>Join this month's swap →</strong></a>
      <br /><br />
    </td>
  </tr>
</table>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## 🔁 This month's Review Swap

<br />

<table width="100%">
<tr>
<td>

**How it works — three steps, five minutes to join:**

1. **Post** — Open this month's pinned swap thread and paste a link to a real, currently-open PR of yours (any language, any repo, doesn't need to be famous). Add 2-3 lines of context: what it does, what kind of feedback you want.
2. **Pick one** — Choose a PR from the thread that doesn't have a reviewer assigned yet, and claim it.
3. **Review for real** — Leave a genuine comment on their PR: one thing that works well, one thing you'd change, one question. Then drop one line back in the thread: `Reviewed → <link>`.

**You owe one review for every PR you post.** That's the only rule. Posting without reviewing back gets a friendly reminder; repeat no-shows are removed from that month's thread so the exchange stays fair for everyone who shows up.

</td>
</tr>
</table>

<br />

<p align="center">
  <a href="../../issues?q=is%3Aissue+is%3Aopen+label%3Areview-swap">
    <img src="https://img.shields.io/badge/→_Join_this_month's_swap-0ea5e9?style=for-the-badge&labelColor=0f172a" />
  </a>
</p>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## 📓 Build Logs

<br />

Each log is one decision, start to finish. Numbered chronologically, newest first.

<br />

| # | Log | Product | Decision | Outcome |
|---|---|---|---|---|
| 007 | [BYOK architecture for AI Chrome extensions: zero backend cost](build-logs/007-byok-chrome-extension-architecture.md) | PR Focus / TabCost | Architecture / AI Integration | ✅ Shipped |
| 006 | [Queue Pattern for `chrome.storage.local` Writes in MV3](build-logs/006-queue-pattern-for-storage-writes.md) | TabCost Pro | Concurrency / Manifest V3 | ✅ Shipped |
| 005 | [Freemium without dark patterns: designing the free tier limits](build-logs/005-freemium-model-design.md) | PR Focus / ChainTrace | Business Model / UX | ✅ Shipped |
| 004 | [MV3 migration: why I moved to Manifest V3 and what broke](build-logs/004-mv3-migration.md) | PR Focus | Migration | ✅ Shipped |
| 003 | [Gumroad vs alternatives: why I picked Gumroad for payments](build-logs/003-gumroad-vs-alternatives.md) | PR Focus | Pricing | ✅ Shipped |
| 002 | [Why PR Focus is BYOK instead of a hosted AI backend](build-logs/002-why-pr-focus-is-byok.md) | PR Focus | Architecture | ✅ Shipped, holding up |
| 001 | [Why PR risk scoring is a hybrid, not a pure AI verdict](build-logs/001-pr-focus-risk-scoring.md) | PR Focus | Architecture | ✅ Shipped, holding up |

<br />

<sub>New logs are added as real decisions happen — not on a fixed schedule, because manufactured weekly content is exactly what this repo exists to avoid. This table is updated by hand with each new log; the Reviewers Board below is the only part of this README generated automatically.</sub>

<br />

<details>
<summary><b>Want to suggest a topic for a future log?</b></summary>
<br />

Open an issue using the <code>Build log proposal</code> template. Good candidates: a decision you're curious about, a tradeoff you disagree with, something from <a href="https://github.com/projekta2/chaintrace-landing">ChainTrace</a>'s pure-JS OOXML writer, or TabCost's idle-detection heuristics.
<br /><br />
</details>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## 🗺️ Roadmap

<br />

Curious about what's coming next? Check the [Roadmap](ROADMAP.md) for planned Build Logs and improvements to the Review Swap. I don't set dates — I write when I ship something worth breaking down.

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

## 📈 Recent Activity

<br />

| Date | Activity | Details |
|---|---|---|
| 2026-06-24 | 📓 Build Log #005 published | [Freemium without dark patterns: designing the free tier limits](build-logs/005-freemium-model-design.md) |
| 2026-06-24 | 📓 Build Log #004 published | [MV3 migration: why I moved to Manifest V3 and what broke](build-logs/004-mv3-migration.md) |
| 2026-06-24 | 📓 Build Log #003 published | [Gumroad vs alternatives: why I picked Gumroad for payments](build-logs/003-gumroad-vs-alternatives.md) |
| 2026-06-22 | 📓 Build Log #002 published | [Why PR Focus is BYOK instead of a hosted AI backend](build-logs/002-why-pr-focus-is-byok.md) |
| 2026-06-20 | 📓 Build Log #001 published | [Why PR risk scoring is a hybrid, not a pure AI verdict](build-logs/001-pr-focus-risk-scoring.md) |
| 2026-06-20 | 🔁 Review Swap opened | [Join the thread](https://github.com/projekta2/build-logs/issues/1) |

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## 🏆 Reviewers Board

<br />

<p align="center"><sub>Everyone who has completed at least one Review Swap. Counts are pulled from the monthly archive after each thread closes — see <a href="review-swap/archive/">review-swap/archive/</a> for the source data behind every number.</sub></p>

<br />

<!-- REVIEWERS-BOARD:START -->
| Reviewer | Swaps completed | Last active |
|---|:---:|---|
| _Be the first — open this month's swap issue_ | — | — |
<!-- REVIEWERS-BOARD:END -->

<br />

<p align="center">
  <sub>Each closed thread is auto-archived to <code>review-swap/archive/</code> by <code>.github/workflows/archive-review-swap.yml</code>, which parses <code>Reviewed → &lt;link&gt;</code> lines from the comments. The archive is best-effort (free-text threads are messy) and explicitly flagged as such in each file — the table above is updated from the archive, not auto-written, so a human always checks the pairings before they're called final. See <a href="CONTRIBUTING.md">CONTRIBUTING.md</a> for the full mechanics.</sub>
</p>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## Why this exists

<br />

I'm not trying to build an audience for its own sake. I'm an indie developer shipping Chrome extensions — [TabCost](https://github.com/projekta2/tabcost-pro-source) (idle tab cost tracking), [PR Focus](https://github.com/projekta2/pr-focus-landing) (AI-assisted PR triage), [ChainTrace](https://github.com/projekta2/chaintrace-landing) (supply-chain data extraction) — and most of what I learn building them never gets written down anywhere useful.

This repo is two things at once: a public notebook of decisions worth remembering, and a low-friction way to trade real code review with other developers who'd rather get feedback today than wait for a vote that may never come.

If something here happens to make you curious about the products that produced these decisions, the links are above. Nothing here requires you to care about that to get value from participating.

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## Ground rules

<br />

```
  yes   Real PRs only — yours, currently open, any language, any size
  yes   Reviews must be substantive — one strength, one suggestion, one question minimum
  yes   Be kind. Critique the code, not the person
  yes   New here? Read one existing thread before posting so you know the tone
   no   No self-promotion threads outside the designated swap issue
   no   No "review my PR" posts with zero context — give reviewers something to work with
   no   No drive-by "LGTM" reviews — that's not a swap, that's a freebie
```

Full details in [CONTRIBUTING.md](CONTRIBUTING.md).

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0ea5e9,100:1e3a5f&height=2" width="100%" />
</p>

<br />

## About

<br />

<table width="100%">
<tr>
<td width="65%" valign="top">

**Alexandre Iglesias** — visual designer who learned to code because the tools I needed didn't exist yet. I run **Projekta2 Creative Studio** in Girona, Spain, building self-funded Chrome extensions end to end: design, code, ship, support.

<p>
  <img src="https://img.shields.io/badge/Background-UX_%2F_Visual_Design-1e3a8a?style=flat-square" />
  <img src="https://img.shields.io/badge/Approach-Indie_%2F_Self--funded-0ea5e9?style=flat-square" />
  <img src="https://img.shields.io/badge/Location-Girona%2C_Spain-1e3a5f?style=flat-square" />
</p>

</td>
<td width="35%" valign="top">

**Products**

▹ [TabCost](https://github.com/projekta2/tabcost-pro-source) — idle tab cost tracker
▹ [PR Focus](https://github.com/projekta2/pr-focus-landing) — AI PR triage
▹ [ChainTrace](https://github.com/projekta2/chaintrace-landing) — supply chain data extraction

</td>
</tr>
</table>

<br />

<p align="center">
  <a href="mailto:hello@projekta2.com"><img src="https://img.shields.io/badge/hello@projekta2.com-D14836?style=for-the-badge&logo=gmail&logoColor=white" /></a>
  <a href="https://linkedin.com/in/alexiglesias1"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" /></a>
  <a href="https://github.com/projekta2"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" /></a>
</p>

<p align="center">
  <sub><i>Building in public · Girona, Spain · One real decision at a time.</i></sub>
</p>

<br />

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1e3a5f,50:0ea5e9,100:0f172a&height=120&section=footer" width="100%" />
</p>
