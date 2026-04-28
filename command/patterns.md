# COMMAND — Build Patterns
Last updated: 260428

## Tool Split: CC vs Cursor
CC for multi-file builds — walk away, let it run.
Cursor for surgical single-file edits only.
Never use Cursor for anything touching lib/pipeline/
or components/tasks/ — those surfaces are CC territory.

## CLAUDE.md Auto-Read (CRITICAL)
CLAUDE.md at command-app/command-app/CLAUDE.md is auto-read
by CC at session start. CURSOR_BRIEF is retired.
Always specify repo (command-app or command-gl) in CC prompt.
Always list explicit file paths after @codebase.

## Every CC Prompt Ends with Git Commit + Push
Last line of every CC prompt:
  git add [specific files]
  git commit -m "[type]: [description]"
  git push
Jason never manually pushes after a build session.
Never use git add . or git add -A — always specific files.

## Cursor Prompt Pattern
Surgical, single-component prompts beat broad sweeps.
Use explicit DELETE/REPLACE with verification steps.
End every Cursor prompt with git commit + push.

## preflight.ps1 — Run Before Every Push
Location: command-app root
Runs: TypeScript check + missing-module scan
Gate: Never push if preflight fails.

## Playwright Auth Pattern (LOCKED)
Test user: jcameron5206@proton.me
Password: CommandTest2026! (reset 260410)
Env vars: COMMAND_TEST_EMAIL + COMMAND_TEST_PASSWORD in .env.local
Auth method: REST API + localStorage injection
Auth file: e2e/auth.setup.ts
Trace on first retry — Playwright Trace Viewer is standard debug tool.

## Concurrent CC Sessions
Safe when file surfaces confirmed non-overlapping.
Check before every batch:
- lib/pipeline/ files: shared — one session at a time
- components/tasks/: shared — one session at a time
- components/dashboard/: safe from components/canvas/
- app/api/ routes: safe to split by route prefix
- supabase/migrations/: each session gets unique timestamp

## Migration Timestamp Ordering
Use seconds apart to control apply order:
  Phase 2.2 → 20260413120000
  Phase 2.3 → 20260413120001
Never use same timestamp. Never rely on alphabetical.
Always query information_schema before writing migrations.

## Session Eating
When multiple CC instances work same repo simultaneously,
one may commit the other's staged files. Not a bug.
Detection: check git log after any session.
Impact: usually harmless. Prevention: not worth attempting.

## executeTask System Prompt Order (LOCKED)
1. Workspace DNA (## Firm Operating Context)
2. Agent system prompt
3. Skills constraints (if applied)
4. Prior session context (if includedCheckpointId)
5. Context brief (truncates first if over budget)
DNA is never truncated. Context brief truncates first.

## Supabase FK Cascade Rule
Always add ON DELETE CASCADE to workspace_id FKs.
Omitting causes test user accumulation in teardown.

## Skills Empty Allowlist Rule
Empty allowedTools = passthrough, not total block.
Advisory by default until operator configures skills.

## API Key Security Pattern
API keys must go through server-side API routes.
Never write directly to Supabase from client browser.
Never log api_key. Never return in responses.

## Vendor Fetch Timeout
All vendor API calls must have 30-second AbortController timeout.
Hung API hangs entire execution without timeout.

## Stripe Env Var Naming (LOCKED 260413)
One canonical name per price ID. No fallback aliases.
STRIPE_FM_PRICE_ID | STRIPE_PRO_PRICE_ID |
STRIPE_SOLO_PRICE_ID | STRIPE_PRICE_STUDIO |
STRIPE_PRICE_AGENCY
Never add || fallback aliases — they cause silent failures.

## Fail-Safe Not Fail-Open (Stripe)
Unknown price ID → default to 'free' not 'pro'.
Billing errors should deny access, not grant it.

## Rate Limiter Pattern (Serverless)
In-memory rate limiters reset on cold start.
Use Supabase audit_ledger count query for rate limiting
in serverless environments. Counts events in time window.
Replace with Upstash Redis before horizontal scale.

## system_prompt Redaction Pattern
Never store full system_prompt in task_executions.
Store: system_prompt_hash (SHA-256) + system_prompt_length.
Workspace DNA and agent instructions are sensitive.

## Agent Fuzzy Matching (Canvas)
4-tier matching for canvas step agent assignment:
1. Exact display_name match
2. Case-insensitive includes
3. agent_type match
4. null (show warning, do not silently fail)
Never silently leave a step unassigned.

## Brain Push Pattern
At end of every session where anything meaningful happened:
  cd "C:\Users\jdavi\OneDrive\Desktop\GlobalInk Repos\globalink-brain"
  git add command/ gl/
  git commit -m "brain: [brief description]"
  git push
Public mirror syncs in ~16 seconds via GitHub Action.
Never skip — next session starts blind without push.
Verify mirror updated: check Last updated date in state.md.

## GitHub Action Health
Sync Action fires on every push to globalink-brain (private).
Writes to globalink-brain-public (public mirror).
If mirror goes stale >48hrs: check Action run logs for failures.
Node.js 20 deprecation: fix Action before Jun 2 2026.
Add on-failure Brevo alert to catch silent failures early.
