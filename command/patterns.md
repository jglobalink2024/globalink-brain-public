# COMMAND — Build Patterns
Last updated: 260416

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

## Stripe Env Var Naming (LOCKED 260413, UPDATED v9.5)
One canonical name per price ID. No fallback aliases.
All follow STRIPE_{TIER}_PRICE_ID convention:
STRIPE_FM_PRICE_ID | STRIPE_PRO_PRICE_ID |
STRIPE_SOLO_PRICE_ID | STRIPE_STUDIO_PRICE_ID |
STRIPE_AGENCY_PRICE_ID
Never add || fallback aliases — they cause silent failures.
Old names STRIPE_PRICE_STUDIO / STRIPE_PRICE_AGENCY retired in 8635e83.

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

## Self-Learning Agent Pattern
Agent definitions live in .claude/agents/ inside the repo (not global ~/.claude/agents).
This makes them version-controlled, product-specific, and auto-loadable.
Bootstrap pattern: agent reads a JSON baseline on first run, populates it from live data,
  uses it for drift detection on every subsequent run.
Self-patch pattern: agent opens GitHub PRs to propose changes to its own definition.
  Never auto-applies changes to itself without human review.

## Ops Run Log Pattern
Diagnostic agents write structured run logs to docs/ops/YYYY-MM-DD-HH-MM.md.
Each log has YAML frontmatter with key metrics for machine-readable trend extraction.
Committed to git automatically after each run — creates a durable history.
Cleanup rule: prune logs >30 days, keep weekly anchors + RED-health runs + permanent records.
Scheduled: daily at 7:01 AM via Claude Code scheduled tasks.

## Canary Probe Pattern
Don't rely on Vercel "READY" status alone — make real HTTP requests to production.
Three probes: health endpoint (200 + latency), auth rejection (expects 401 not 200),
  primary feature route (500 = crisis). Live proof the app is serving, not just deployed.

## Codebase Drift Detection Pattern
Read filesystem (app/api/) at runtime and compare against a stored baseline.
New routes not in baseline = uncovered monitoring gap → queue for self-patch.
Removed routes still in baseline = dead checks → flag and remove.
Always compare local vs production commit SHA — catches silent deploy failures.

## Chat/Thread Naming Convention (LOCKED 260413, UPDATED 260416)
Name every Claude.ai chat based on the ENTIRETY of the
conversation — not just the opening topic.

CANONICAL FORMAT: [GL | WORKSTREAM | Topic · Topic | YYMMDD]

HARD RULES:
1. Leading namespace is always "GL" — never "COMMAND" or any project name
2. Separator between topics is middle-dot "·" (U+00B7) — never "-" or ","
3. Date = START date of the chat (when it was created/opened — NOT last activity)
   If uncertain about start date, Claude asks Jason before naming
4. Brackets [ ] are required
5. Pipe with spaces " | " separates all sections

VALID EXAMPLES:
✅ [GL | QA | F01-F02 OAuth Fix · Dependabot #8 | 260416]
✅ [GL | OPS | File Lifecycle Policy · Cleanup Agent · Chat Naming Fix | 260416]

INVALID EXAMPLES:
❌ COMMAND | QA | F01-F02 OAuth Fix + Dependabot #8 | 260416  (missing brackets, wrong namespace, wrong separator)
❌ [GL | QA | F01-F02 | 260417]  (wrong date if chat opened 260416)
❌ [GL | COMMAND | Phase 2 Sprint · Audit · Brain Init · 260413]  (wrong namespace)

Never name a chat based only on the opening question.
Never use the current date if the chat started on a different day — always use start date.
Review the full conversation before naming.
Always suggest a thread name when the session is closing out.

## Pending Actions Tracking (LOCKED 260416)
Never bury manual Jason-action deliverables in prose. Always write to
PENDING_ACTIONS.md in the target repo root.

Row format: `- [ ] TYPE | WHERE | WHAT | YYMMDD | SESSION`
Types: SQL | VERCEL | GITHUB | GCP | VERIFY | STRIPE | HUBSPOT | OTHER

CC reads at session open (surface relevant open items), writes at session close
(any new manual actions). Jason checks off. Never delete rows.

## Brain Multi-Variant Sync (LOCKED 260416)
All Claude variants (Code, chat, Cursor) read from and write to the same private
brain repo. No variant is a second-class citizen.

READ priority: GitHub MCP → local filesystem → public mirror fallback
WRITE priority: GitHub MCP (chat) | local git (CC/Cursor) | output block (fallback)

Commit format all variants: `brain: [description] [via: CC | chat | cursor]`
Full protocol: globalink-brain/gl/brain-sync-protocol.md
Setup required: GitHub PAT (contents:write on brain repo) + GitHub MCP in Claude.ai

## File Lifecycle Policy (LOCKED 260416)
Every file Claude creates must be tagged at creation:
[ONE-USE] — delete after 7 days or after the commit that ran it
[EVIDENCE] — keep 90 days, then archive (tests/personas/, docs/ops/)
[PERSISTENT] — keep forever (brain files only)

SESSION_CLOSEOUT_*.md: [ONE-USE], expires 3 days
CC_*.md in repo root: [ONE-USE], expires 7 days post-commit
docs/ops/*.md: [EVIDENCE], archive after 30 days (keep RED-health runs forever)
tests/personas/*: [EVIDENCE], archive after 90 days
globalink-brain/**: [PERSISTENT]

Cleanup enforced by ops-watchdog Step 6 (daily after health checks).
Full policy: globalink-brain/command/file-lifecycle.md

## Testing doctrine — journey-matrix over item-checklist (v12 onward)

**Why the shift**:
v11 scored 56/64 PASS at the item level but missed that the crown-jewel
claim (context handoff) had never been tested end-to-end with real
payload inspection. The item-based approach optimizes for "does the
button exist" when what we actually need is "does the button do what
we claim when a real user clicks it." BILL-02 (phantom Studio tier)
was caught in v11 only because it happened to fall inside an item
check — a journey-based approach would have caught it on persona P2's
first conversion attempt.

**The new doctrine**:

1. Tests are organized by PERSONA × JOURNEY × CLAIM, not by feature or
   page.
2. Every assertion is a POST-ACTION observation. Code reading is not
   evidence. DOM presence is not evidence. Endpoint 200 is not evidence.
3. Four legitimate verification methods:
   - Screenshot comparison (pre-action vs post-action)
   - Network payload inspection (request AND response body)
   - DOM state at t+Ns post-action
   - Backend query or credit-balance delta
4. Personas have BEHAVIORAL CONSTRAINTS (what they'd do, what would
   make them abandon). Orchestrator must honor these — if Marcus
   would never open DevTools, the orchestrator running as Marcus
   cannot use DevTools.
5. Abandon signals are first-class outcomes. A product that ships
   with a P1 abandon signal is broken for Iris, even if every assertion
   passes.

**What this doctrine does NOT replace**:
- CC (Claude Code) multi-file build sessions still use item-level
  verification (TypeScript exit 0, preflight PASS, ESLint 0)
- Unit tests stay item-level
- Migration audits stay item-level
- Security reviews stay item-level

**What it replaces**:
- Any pre-beta QA pass
- Any conversion-path audit
- Any customer-facing claim validation

**Scoring rubric**:
- ≥95% PASS → SHIP
- 85–94% with 0 CRITICAL on C3/C4/C6 → SHIP WITH FIXES
- <85% OR any CRITICAL on C3/C4/C6 → DO NOT SHIP

C3 (handoff) is weighted 2x. Context handoff failure is product-thesis
failure — no other score can compensate.

---

## File locations

Canonical artifacts for v12:
- globalink-brain/command/symphony/v12/COMMAND_Personas_v12.md
- globalink-brain/command/symphony/v12/COMMAND_Journeys_v12.md
- globalink-brain/command/symphony/v12/COMMAND_Claims_v12.md
- globalink-brain/command/symphony/v12/COMMAND_Symphony_v12_Prompt.md

Future symphonies (v13+) inherit this structure. Personas and
journeys evolve as the product grows; claims evolve as marketing
evolves. The doctrine — click-then-observe — does not evolve.

---

## Persona roster (v12 baseline)

- P1 Iris: Non-technical stakeholder, low fluency, Jason's wife
- P2 Eric: Actual P1 beta target, medium-high fluency, real customer
- P3 Danielle: Primary ICP (RevOps at boutique consulting), medium-high
- P4 Marcus: Tertiary ICP (solo coach), low-medium fluency, warm outbound

Additions for later symphonies (suggest, do not add without Jason's green-light):
- P5 Danny Suk Brown: RAP-flagged real outbound target
- P6 LATAM operator (post-May 1)
- P7 Enterprise buyer (Phase 3, not MVP)

---

## Journey roster (v12 baseline)

- J1 First dispatch (happy path, zero-key)
- J2 Multi-agent handoff with context transfer (crown jewel)
- J3 Failure recovery
- J4 Returning user / state persistence
- J5 Pilot → paid conversion

Journeys NOT YET IN DOCTRINE (candidates for v13+):
- J6 BYOK configuration flow (standalone, currently folded into J3)
- J7 Team/workspace invitation flow (if multi-seat ships)
- J8 MCP server registration (currently API-only, tested in J2.7)

---

## Anti-patterns (explicit DO NOTs)

- ❌ Do not run v12 against synthetic personas when real outbound
   targets exist — stay grounded in real buyer profiles
- ❌ Do not run v12 without real credit burn — "simulated" tests are
   not v12 tests
- ❌ Do not skip the persona entry/exit protocol — that's the
   behavioral enforcement layer
- ❌ Do not weight C3 equally with other claims — it's product-thesis
- ❌ Do not let a "mostly passed" verdict become a ship decision —
   the rubric is categorical for a reason
- ❌ Do not re-run a full symphony to verify a single fix — use
   a surgical v12.1 patch covering only the failed cells

---

## Commit convention

When committing symphony artifacts:
```
brain: symphony v[N] [complete|in-progress|verdict]
```

When committing doctrine updates to patterns.md:
```
brain: patterns — [short description of doctrine change]
```
