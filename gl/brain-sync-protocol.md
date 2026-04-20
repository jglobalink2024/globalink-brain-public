# GlobaLink Brain — Multi-Variant Sync Protocol
Last updated: 260416

## Intent

Every Claude variant (Code, chat, Cursor) reads from and writes to the same
brain. Single source of truth. No variant is a second-class citizen.
Jason is never the action officer for syncing.

---

## Access Architecture

```
Private brain repo (jglobalink2024/globalink-brain)
       ↑↓ git push/pull                         ↑↓ local files + git
   Claude Code                                      Cursor
  (local filesystem)                           (local filesystem)
          ↑
          | git commit (ops-watchdog drains queue daily at 7:01 AM)
          |
  Supabase brain_queue table  ←── INSERT ───  Claude chat (claude.ai)
  (ycxaohezeoiyrvuhlzsk)           Supabase connector (already installed)
          ↓
  Public mirror (~16s after each push)
  jglobalink2024/globalink-brain-public
  (read-only fallback for session opens)
```

**Write latency for Claude chat:** max 24 hours (next watchdog run at 7:01 AM).
Jason can trigger `/ops-check` at any time for an immediate drain.

---

## READ Protocol (all variants)

**Priority order:**
1. **Local filesystem** (CC / Cursor) → `C:\Users\jdavi\OneDrive\Desktop\GlobalInk Repos\globalink-brain\`
2. **GitHub Integration connector** (Claude chat) → attach brain files from private repo as context
3. **Fallback** → public mirror URLs (raw.githubusercontent.com/jglobalink2024/globalink-brain-public/main/...)

**Session Open — files to read:**
```
command/state.md
command/decisions.md
command/patterns.md
command/killed.md
command/file-lifecycle.md
gl/principles.md
gl/brain-sync-protocol.md   ← this file
```

---

## WRITE Protocol

### Claude Code / Cursor (synchronous — immediate)
1. Write/edit local files in `globalink-brain\`
2. `git add <specific files>` (never `git add .`)
3. `git commit -m "brain: [description] [via: CC]"`
4. `git push`
5. Public mirror syncs automatically (~16s)

### Claude chat (async — via Supabase queue)
At session close, for each brain file that needs updating, INSERT into `brain_queue`:

```sql
INSERT INTO brain_queue (file_path, operation, section_header, content, via)
VALUES (
  'command/state.md',          -- target file relative to brain repo root
  'append',                    -- 'append' or 'full_replace'
  'Next Session Priorities',   -- ## header to append after (null = end of file)
  '[the content to add]',      -- the actual update text
  'chat'                       -- always 'chat' for Claude chat sessions
);
```

**ops-watchdog drains the queue** at 7:01 AM daily (Step 7):
- Reads all `WHERE committed_at IS NULL` rows
- Applies each update to the brain file (append or replace)
- Commits to git: `brain: drain queue — [N] update(s) [via: watchdog←chat]`
- Marks rows as committed with the git SHA

**Trigger immediate drain:** run `/ops-check` — watchdog executes Step 7 on demand.

### Commit format (all variants)
```
brain: [description] [via: CC | cursor | watchdog←chat]
```

---

## brain_queue Table Schema

```sql
-- Table: brain_queue (in Supabase project ycxaohezeoiyrvuhlzsk)
id            uuid        PK, auto
created_at    timestamptz DEFAULT now()
file_path     text        -- relative path in brain repo, e.g. 'command/state.md'
operation     text        -- 'append' | 'full_replace'
section_header text       -- for append: ## header to insert after (null = end of file)
content       text        -- the content block to write
via           text        -- 'chat' | 'cursor' | 'CC'
committed_at  timestamptz -- null = pending; set when watchdog commits
commit_sha    text        -- git SHA after commit
```

Migration: `command-app/supabase/migrations/20260416100000_brain_queue.sql`
RLS: disabled — internal ops table, not customer data.

---

## SESSION CLOSE (all variants)

Before announcing session complete:

1. Identify which brain files changed this session
2. Write updates via the available path:
   - CC/Cursor: local file → git push (immediate)
   - Chat: INSERT into brain_queue (async, committed by watchdog)
3. Confirm commit SHA (CC) OR confirm INSERT succeeded (chat)
4. Suggest thread name per canonical format

Never announce "done" until brain updates are queued or committed.

---

## SETUP STATUS

| Variant | Read | Write | Setup needed |
|---------|------|-------|-------------|
| Claude Code | Local filesystem | git push | None — works now |
| Cursor | Local filesystem | git push | None — works now |
| Claude chat | GitHub Integration (read-only) + public mirror | Supabase brain_queue INSERT | Apply migration in SQL Editor |

**One-time Jason action:** Apply `20260416100000_brain_queue.sql` in Supabase SQL Editor.
That's the only setup step remaining. Everything else is already wired.

---

## ANTI-PATTERNS (never do these)

- Never write brain content to CC local memory (`C:\Users\jdavi\.claude\projects\...`)
- Never skip the close commit/queue because "it was a small session"
- Never use `git add .` to stage brain commits — specific files only
- Never commit to `globalink-brain-public` directly — it is auto-synced, not manually written
- Never write GL entity-sensitive content (`gl/entities.md`) via the public mirror path
- Never use the "output block fallback" (Jason as action officer) — Supabase queue replaces it
