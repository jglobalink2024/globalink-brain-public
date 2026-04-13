# COMMAND — Build Patterns
Last updated: 260413

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
