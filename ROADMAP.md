# Roadmap — Build Logs

This is a living document. It reflects what I'm thinking about, not a promise list. Priorities shift when I ship things or learn something new.

---

## 📓 Build Logs — What I'm considering writing next

These are real decisions I've faced while building Chrome extensions. I'll write about whichever one feels most useful at the time.

| Topic | Why it might be useful | Status |
|---|---|---|
| **MV3 migration — what broke and how I fixed it** | Manifest V3 changed persistence patterns. I hit several issues moving from background pages to service workers. Could save others the same headaches. | 🟡 Next up |
| **BYOK UX — API key validation, error states, and user feedback** | This is the biggest friction point in PR Focus. I've learned a lot about what users get wrong and how to guide them. | 🟡 Next up |
| **How I built a pure-JS OOXML writer for ChainTrace** | I wrote a small library to generate real .xlsx files without external dependencies. It's a niche topic but might be useful for extension devs. | 🔵 Idea |
| **Gumroad licensing without a backend** | I validate licenses entirely client-side using signed JWTs and public keys. It's not perfect, but it works for a solo dev. | 🔵 Idea |
| **Prompt engineering for consistent risk scoring** | I've iterated on prompts to reduce variance between calls. Some things worked, some didn't. | 🔵 Idea |

---

## 🔁 Review Swap — What I'd like to improve

The Review Swap is live and working. These are things I'd like to add, but only if they make sense and don't add complexity for the sake of it.

| Feature | Why | Status |
|---|---|---|
| **Monthly thread automation (GitHub Actions)** | Auto-opens and closes threads each month. | ✅ Done |
| **Reviewers Board (manual updates)** | Recognizes participants. | ✅ Done |
| **Automated archive generation** | Preserves each month's thread for transparency. | ✅ Done |
| **Semi-automated Board updates** | Would save me 10 minutes/month, but only if I can keep it reliable. | 🟡 Considering |
| **Notification reminders** | Would help participants remember to review back, but I don't want to spam. | 🔵 Idea |

---

## 🚀 Community growth — not a goal, a side effect

I'm not chasing numbers. If the Build Logs and Review Swap are genuinely useful, growth will come. That said, these are things I'd notice:

| What I'd notice | Status |
|---|---|
| Someone other than me posts a PR in the swap | 🔜 Waiting — this is the real validation |
| Someone suggests a guest Build Log | 🔜 Waiting |
| Someone tells me a Build Log helped them make a decision | 🔜 Waiting |

---

## 📌 Why no dates?

Because I'm a solo developer with three products in production. I write Build Logs when I ship something interesting or learn a lesson worth sharing, not on a schedule. Dates would be a promise I can't keep.

---

*This roadmap is updated when I actually change my mind, not on a fixed schedule.*
