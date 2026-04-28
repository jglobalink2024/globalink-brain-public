# GlobaLink Brain — COMMAND Project
# Add this file to Claude.ai project knowledge.
# Claude fetches these URLs at session start automatically.

---

## 🛡️ MANDATORY — Brain Freshness Gate (run FIRST, before anything else)

Before any work begins in a non-trivial session, Claude executes
this gate. If the brain is stale, work pauses and Jason is alerted.

### Step 1 — Fetch state.md with cache-bust

Fetch this URL (the query parameter forces a fresh CDN read):

https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/state.md?nocache=session-start

If web_fetch returns content from earlier in the conversation,
treat that as a tooling cache hit and re-fetch with a different
query param (e.g. ?nocache=retry1).

### Step 2 — Parse the Last updated line

The first 5 lines of state.md contain: `Last updated: YYMMDD`
Extract that date. Convert to a real date.

### Step 3 — Check freshness

Compare to today's date.

| Age | Action |
|---|---|
| ≤ 48 hours | ✅ Proceed normally. No banner needed. |
| 49h – 7 days | 🟡 Display soft banner at top of first response. Continue work. |
| > 7 days | 🔴 Display hard banner. Recommend catchup BEFORE work begins. |

### Step 4 — Banner format

Soft banner (49h – 7d):
> 🟡 Brain is N hours stale. Last push: YYMMDD. Consider a
> brain push at session close.

Hard banner (>7d):
> 🔴 BRAIN STALE — N days since last push. Mirror may not
> reflect recent decisions. Recommend running CC catchup
> before strategic work. Want me to draft the catchup prompt?

### Step 5 — On gate failure

If the fetch itself fails (404, timeout, no network), do NOT
silently proceed. Display:

> ⚠️ Could not verify brain freshness. Fetch failed: [error].
> Proceeding with hot memory only — flag any decisions for
> manual brain push at session close.

---

## 📥 Fetch list (after freshness gate passes)

Append `?nocache=session-start` to all URLs to force fresh CDN reads.

https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/state.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/decisions.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/patterns.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/killed.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/command/research.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/gl/principles.md?nocache=session-start
https://raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/gl/decisions.md?nocache=session-start

---

## 🎯 Session type guidance

BUILD SESSION — fetch: state.md, patterns.md, killed.md
STRATEGY SESSION — fetch: state.md, decisions.md, research.md
GTM SESSION — fetch: state.md, decisions.md + paste gl/entities.md
  (entities.md is private — Jason pastes it manually)
COMPETITIVE SESSION — fetch: research.md, killed.md
PATENT SESSION — fetch: killed.md, decisions.md, research.md

---

## 🗂️ Brain location (local)

C:\Users\jdavi\OneDrive\Desktop\GlobalInk Repos\globalink-brain\

## 🔒 Private repo (CC writes here)

https://github.com/jglobalink2024/globalink-brain

## 🌐 Public mirror (Claude.ai reads here)

https://github.com/jglobalink2024/globalink-brain-public

## 🔄 Sync

Automatic GitHub Action fires on every push to private repo.
Public mirror updates in ~16 seconds.

If the freshness gate trips, common root causes:
1. Local files written but never git-pushed
2. Sync Action failing (check: gh api .../actions/runs)
3. Public mirror archived (check: gh repo view ...)
4. Action script bug (rare)
