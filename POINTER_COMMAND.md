# GlobaLink Brain — COMMAND Project
# Add this file to Claude.ai project knowledge.
# Claude fetches these URLs at session start automatically.

---

## 🛡️ MANDATORY — Brain Freshness Gate (run FIRST, before anything else)

Before any work begins in a non-trivial session, Claude executes
this gate. If the brain is stale, Chat does not just alert — it
synthesizes the catchup delta and presents a ready-to-fire CC
prompt for approval.

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
| 49h – 7 days | 🟡 Soft banner + auto-catchup synthesis below |
| > 7 days | 🔴 Hard banner + auto-catchup synthesis BEFORE any work |

### Step 4 — Banner + auto-catchup synthesis

Soft banner (49h – 7d):
> 🟡 Brain is N hours stale. Last push: YYMMDD.
> Auto-catchup drafted below — approve to fire, edit, or skip.

Hard banner (>7d):
> 🔴 BRAIN STALE — N days since last push.
> Recovery is non-optional. Auto-catchup drafted below.
> Approve to fire CC, edit the delta, or "skip" to proceed
> with hot memory only (logs as IP_violation).

### Step 5 — Auto-catchup synthesis (when stale)

When the freshness gate trips, Chat must:

1. Scan hot memory for changes dated after the brain's
   Last updated value
2. Scan the current conversation history for session content
   that should land in brain (decisions made, bugs found,
   features shipped, research locked)
3. Categorize changes by file:
   - state.md → build phase, GTM pipeline, active bugs,
     pending manual steps (full-replace mode)
   - decisions.md → architectural/IP decisions (append-only)
   - patterns.md → new build conventions (append-only)
   - killed.md → ideas/approaches rejected (append-only)
   - research.md → market/competitive intel (append-only)
4. Draft per-file deltas tagged with append-only or
   full-replace mode
5. Format the output as a ready-to-execute CC prompt
   that routes through brain-committer
6. Present the prompt for Jason's one-click approval
   before any other work begins

The synthesis output template:

> 🔄 AUTO-CATCHUP DRAFT
>
> Brain age: N days. Detected delta from hot memory + session.
>
> [Per-file deltas, tagged FULL-REPLACE or APPEND]
>
> Approve to fire CC. Reply "edit" to revise. Reply "skip" to
> proceed without push (logged as a Rule 12 risk).

### Step 6 — On gate failure

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

## 🛡️ Rule 12 — Brain Write Routing

Any write to globalink-brain/command/ or globalink-brain/gl/
MUST route through the brain-committer agent.
Direct writes are a Rule 12 violation.

Per-file mode defaults:
  state.md          FULL-REPLACE (snapshot)
  gl/principles.md  FULL-REPLACE (doctrine snapshot)
  decisions.md      APPEND (log of decisions)
  killed.md         APPEND (log of kills)
  research.md       APPEND (log of intel)
  patterns.md       APPEND with in-place edits (edits flagged)
  gl/decisions.md   APPEND (log of cross-entity decisions)
  gl/entities.md    NEVER write via agent or directly (private)

Exceptions:
  .github/workflows/ — outside brain-committer scope, direct
                       writes allowed with manual git discipline
