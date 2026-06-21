# Launch Checklist — Build Logs

Step-by-step to get this repo from files-on-disk to fully live and working. Follow in order — later steps depend on earlier ones.

---

## Phase 1 — Repo setup (≈20 min)

- [ ] **1.1** Create a new **public** repo on GitHub named `build-logs` under your `projekta2` account/org.
  - Do NOT initialize with a README, license, or .gitignore on GitHub's side — you already have all of these locally, and letting GitHub create its own copies will cause merge conflicts on first push.

- [ ] **1.2** From the folder containing all the files prepared here, run:
  ```bash
  cd build-logs
  git init
  git add .
  git commit -m "Initial commit: Build Logs + Review Swap launch"
  git branch -M main
  git remote add origin https://github.com/projekta2/build-logs.git
  git push -u origin main
  ```

- [ ] **1.3** On GitHub, go to **Settings → General** and:
  - Add a short description: `Real engineering decisions from shipping 3 Chrome extensions solo, plus a monthly free code review swap.`
  - Add topics: `code-review`, `indie-hacker`, `chrome-extension`, `developer-tools`, `build-in-public`
  - Add the website link: your PR Focus landing page (or a generic Projekta2 page if you have one)

- [ ] **1.4** Go to **Settings → Features** and confirm **Issues** is enabled (it is by default on public repos, but verify). **Discussions** stays off for now — Issues alone are the entire mechanism, adding Discussions too early just splits your (currently small) audience across two systems.

- [ ] **1.5** Don't be alarmed if the stars/license badges at the top of the README show "repo not found" or "invalid" for the first few minutes after pushing — shields.io needs the repo to be public and indexed before those dynamic badges resolve. They self-correct; no action needed.

---

## Phase 2 — Labels (≈5 min)

GitHub's default label set doesn't include what this repo needs. Create these manually under **Issues → Labels → New label**:

| Label | Color (hex) | Description |
|---|---|---|
| `review-swap` | `#2563EB` | The monthly Review Swap thread |
| `build-log-proposal` | `#0EA5E9` | A proposed guest Build Log |
| `question` | `#94A3B8` | General questions about how this works |

---

## Phase 3 — Branch protection (≈3 min)

Since the archiver workflow pushes commits directly to `main` when a swap thread closes, you do **not** want strict branch protection requiring PR review on `main` — that would break the automation. Leave `main` unprotected, but:

- [ ] **3.1** Go to **Settings → Actions → General → Workflow permissions** and select **"Read and write permissions"** — the archiver needs this to commit the archive file back to the repo. Without this single setting, it will fail silently the first time a thread closes.

---

## Phase 4 — Verify the automations work (≈10 min)

Don't trust the workflows blindly — test them before announcing anything.

- [ ] **4.1** Go to the **Actions** tab and confirm **"Archive Review Swap Thread"** shows up without a YAML syntax error (GitHub flags invalid workflow YAML immediately on push — a red "invalid workflow file" banner means something needs fixing before you go further). This workflow only *triggers* on issue close, so you can't fully test it yet — that happens naturally in Phase 6 when the first thread closes.

---

## Phase 5 — Publish the first Review Swap thread (≈5 min)

- [ ] **5.1** Go to **Issues → New issue**, select the **"💬 Review Swap — Monthly Thread"** template.
- [ ] **5.2** Edit the title to the actual current month, e.g. `Review Swap — June 2026`.
- [ ] **5.3** Publish it.
- [ ] **5.4** **Pin it** (top-right of the issue page, "Pin issue") so it's the first thing anyone sees on the Issues tab.
- [ ] **5.5** Post the first PR yourself, using the format in the template, with a real open PR of yours (it can be a small one — the point is to seed the format, not to be the perfect example).

---

## Phase 6 — Monthly maintenance loop (ongoing, ≈15 min/month)

This is the only recurring work, and it fits inside your existing 1-2h/week budget:

- [ ] **6.1** On the **last day of the month**, close the open Review Swap issue. The archiver workflow triggers automatically and commits a new file to `review-swap/archive/`.
- [ ] **6.2** Within a day or two, **spot-check** the generated archive file — confirm the post/review pairings look right (remember: this matching is best-effort, see the note inside `archive-review-swap.yml`).
- [ ] **6.3** Manually update the **Reviewers Board** table in the root `README.md` based on the archive — this is deliberately not automated (attribution is a judgment call, better made by you than by a brittle script reading free-text comments).
- [ ] **6.4** Open a **new** Review Swap issue from the template for the new month, pin it, unpin the old one.
- [ ] **6.5** If a new Build Log went live this month, add its row to the table in the root `README.md` by hand (see Phase 4 note above — this table is intentionally not auto-generated).

---

## Phase 7 — Cross-promotion (one-time + ongoing, fits your existing kit)

- [ ] **7.1** Add a line to the README of your three product repos (`tabcost-pro-source`, `pr-focus-landing`, and ChainTrace's repo) pointing back here — something like: *"I also write about the engineering decisions behind this in [Build Logs](https://github.com/projekta2/build-logs)."* One line, not a banner — this stays low-key by design.
- [ ] **7.2** When you publish the first log, that's a natural, non-salesy dev.to / Reddit post: "I started logging the real trade-offs behind shipping 3 Chrome extensions solo — here's the first one, on why PR Focus doesn't just sort by AI score." This reuses the PR Focus launch kit's Week-1 "tell the story" approach, just pointed at this repo instead.
- [ ] **7.3** Do **not** announce the Review Swap thread on Reddit/HN until it has at least 2-3 real participants (you + a couple of people you know, or early repo visitors). An empty swap thread looks worse than no thread — wait for it to have visible activity before pushing traffic at it.

---

## What "done" looks like

By the end of Phase 5, you have: a public repo with one real Build Log live, a working archiver workflow ready for the first thread close, an open and pinned Review Swap thread, and clear contribution rules. That's a fully functional v1 — everything in Phase 6 onward is just running it, not building it.
