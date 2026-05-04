[PERSISTENT]
# COMMAND — Current State
Last updated: 260504
Author: CC

## Live URLs
App: app.command.globalinkservices.io
Landing: command.globalinkservices.io
Sales Hub: command.globalinkservices.io/sales-hub
Beta Syllabus: command.globalinkservices.io/beta-syllabus
NDA: go.command.globalinkservices.io/betaNDA
Demo: go.command.globalinkservices.io/demo (Navattic)
Stripe FM: https://buy.stripe.com/9B68wRgwAcp8dt2dJp9k407
Stripe Pro: https://buy.stripe.com/bJe00lfsw74O74E5cT9k408
Documenso: https://documenso-gl.onrender.com

## Build State
Phase 1 + 1.5: COMPLETE
Phase 2: COMPLETE (all 15 sub-phases + MCP Host MVP shipped)
Phase 3: NOT STARTED (gate: first paying customer)

## Phase 2 — All Shipped
2.1 Real agent connections (Anthropic + OpenAI + Perplexity)
2.2 Task Execute Engine
2.3 Auto-Handoff + Chain + wire
2.4 Router → Execution (75% threshold, 4s override)
2.5 Skills Enforcement + Agent Guardrails
2.6 Canvas Linear Execution
2.7 Semantic Matchmaking schema + scorer
2.8 Stripe E2E + plan gating
2.X Workspace DNA
2.Z-a Google Workspace OAuth
2.Z-b HubSpot OAuth
2.4.5 Smart Suggestions
2.Y RevOps Canvas Templates
2.9 ROI Tracker
MCP Host MVP (commit 2e2924a):
  /api/mcp/register
  /api/mcp/status
  lib/mcp/updateAgentStatus.ts (shared utility)
  Auto-CCF checkpoint on task_complete
  Settings > Integrations MCP panel
  Violet [MCP] badge on agent cards

## Documenso — LIVE 260504
URL: https://documenso-gl.onrender.com
Render service: srv-d7b9i69r0fns73fgsgu0
Last deploy: dep-d7s8n228qa3s73e09hdg — LIVE as of 2026-05-04 08:06 EDT
Start command (LOCKED): `cd apps/remix && node build/server/main.js`
  Root cause of prior failures: Turbo `persistent: true` tasks silently no-op in non-TTY (Render CI)
  CWD prefix also fixes Hono serveStatic path (looks for build/client relative to CWD)
Pre-Deploy Command: paid Render feature — NOT available on free tier
  Schema sync must be done manually in Supabase SQL Editor when Prisma schema changes
  Current schema is in sync — no action needed unless Documenso schema changes
Supabase: project ycxaohezeoiyrvuhlzsk, schema `documenso`, all Prisma tables in sync
pg_trgm extension: moved to `documenso` schema (already done)
Static assets: working (fixed by `cd apps/remix &&` CWD prefix)
Skill document: ~/.claude/agents/documenso-operator.md

## Infra Audit 260416 — COMPLETE
40 migrations applied
7 env vars confirmed
NEXT_PUBLIC_GL_INTERNAL removed
Migration log updated
CLI injection reported to Vercel

## Session Log

### 260504 — Documenso Deploy · Render Turbo Fix
[PERSISTENT]
Last updated: 260504
Author: CC
Session: [GL/COMMAND | INFRA | Documenso Deploy · Render Turbo Fix | 260504]
- Documenso v2.8.1 now LIVE at https://documenso-gl.onrender.com
- Root cause resolved: `turbo run start --filter=@documenso/remix` silently no-ops in Render CI (non-TTY, persistent:true tasks)
- Fix: Start command changed to `cd apps/remix && node build/server/main.js` — bypasses Turbo entirely
- CWD prefix also fixes Hono serveStatic static asset resolution (build/client relative to app dir)
- Pre-Deploy Command (prisma db push) confirmed paid-tier Render feature — not available on free tier
- Schema sync manual going forward; current schema confirmed in sync
- documenso-operator.md agent skill written to ~/.claude/agents/

### 260504 — Documenso NDA Template Fields + Test Send
[PERSISTENT]
Last updated: 260504
Author: CC
Session: [GL/COMMAND | OPS | Documenso NDA Template Fields · Test Send | 260504]
- Added 5 fields to Documenso template 12572 (COMMAND Beta NDA v2.0), page 4, Participant right column
- Fields: SIGNATURE(id:10520397,y:12.5), NAME(id:10520396,y:15.8,required:true), TEXT Company/Org(id:10520398,y:18.6,required:true), TEXT Title/Role(id:10520399,y:21.4), DATE(id:10520400,y:24.2)
- All mapped to recipient 2114590 (recipient.1@documenso.com, SIGNER)
- Discovery: tRPC proc=field.setFieldsForTemplate; auth header x-team-id:178745; envelopeItemId=envelope_item_lfevxkkomiolhlal (templateDocumentData)
- Test invite sent: doc 1260791 to jcameron5206@proton.me — signing URL https://app.documenso.com/sign/fKrJUdfR5ieBqkBtcHK4g — Full Name + Signature confirmed visible in signing UI
- PENDING_ACTIONS.md Documenso row marked [x]

### 260504 — P1 #4 MCP SQL Migrations + APPSMOKE-06 Fix
[PERSISTENT]
Last updated: 260504
Author: CC
Session: [GL/COMMAND | BUILD | MCP SQL Migrations · APPSMOKE-06 Fix | 260504]
- P1 #4 MCP SQL Migrations shipped to production Supabase (ycxaohezeoiyrvuhlzsk)
- Three columns added: mcp_secret TEXT NOT NULL (entropy default) on workspaces; mcp_endpoint_url TEXT nullable and capabilities JSONB nullable on agents
- JSONB column type change required DROP + recreate of agents_safe view
- lib/supabase/types.ts regenerated via Supabase MCP
- tsc: exit 2 → exit 0 after fixing type debt in 5 files (CheckpointRow, Checkpoint interface, command-data.ts, ledger.ts entry_seq, dev/reset dynamic table cast)
- APPSMOKE-06 fixed: /GlobalInk/i case-insensitive flag falsely matched correct footer text "GlobaLink LLC" (both fold to "globalink"). Fix: dropped "i" flag → case-sensitive
- Committed c7e6de3, pushed to main

### 260503 — Brain Hardening #3 — ntfy.sh second alert channel
Session: [GL/COMMAND | OPS | Brain Hardening · ntfy.sh Alert Channel | 260503]
- F2 + F8b closed: ntfy.sh added as fully independent second alert channel alongside Brevo
- NTFY_TOPIC secret added to globalink-brain GitHub repo (long random topic, unguessable)
- sync-public.yml: existing step renamed "Notify on failure (Brevo email)"; new parallel step "Notify on failure (ntfy.sh push)" added with `if: failure()`, Priority: urgent
- brain-heartbeat.yml: ntfy curl added inside `AGE_HOURS > 48` branch before `exit 1`, Priority: high
- Both channels verified independent: different API, different secret, different transport
- Live test: injected `exit 1` as first step → run 25297494565 failed → Brevo messageId <202605040203.16871763278@smtp-relay.mailin.fr> + ntfy id YaeDaKZglqcm both confirmed HTTP 200
- Revert confirmed: run 25297521970 succeeded — workflow back to production state
- Jason manual step remaining: install ntfy app + subscribe to topic gl-brain-ops-25a9465b49b52d152e14aa5d0f071c5e

### 260428 — Layer 3.5 Config Version Control + POINTER v3 deploy
Session: [GL/COMMAND | OPS | POINTER v3 Deploy · Config Backup Repo | 260428]
- D1 (2a): POINTER_COMMAND.md v3 confirmed in globalink-brain (commit 38c8e81 from prior session); Downloads v3 matched repo exactly — no new commit needed
- D1 (2b): POINTER_COMMAND.md v3 uploaded to claude.ai COMMAND project knowledge — React fiber injection + POST /docs 201. 163 lines, hasAutoCatchup:true, persistent via API GET. Old 162-line copy deleted; 2 accidental duplicates cleaned.
- D1 (2c): cache-bust curl confirmed "Auto-catchup synthesis" in globalink-brain-public — ✅
- D2: globalink-claude-config private repo created (github.com/jglobalink2024/globalink-claude-config) — 207 agents + 2 hooks + settings.json + .mcp.json. Commits caa7429 + 8e14ace. No secrets found (pre-add grep verified). Option B sync: ~/bin/claude-config-sync.sh live + tested. Hooked into ~/bin/closeout.

### 260428 — Retroactive 260418 brain writes (GTM motion + pricing)
Session: [GL/COMMAND | BRAIN OPS | GTM Motion · Pricing Decision | 260428]
- state.md: Last updated set to 260418; FM Cohort section + GTM motion/content/Eric gate lines added
- decisions.md: 2 entries appended — community-first replaces Waalaxy top-of-funnel; flat+caps+overages pricing (Sandra mental model, $29 Starter, FM $99 preserved)
- patterns.md: content system pattern appended — batch/schedule/template/co-write/byproduct, no blank-page drafting
- killed.md: pure usage-based pricing as primary model killed — Row 3 pricing for Row 3 buyers; cap+overage solves margin
- Commit: a5d0204 | pushed to main

### 260428 — Brain catchup push + RAP-B SessionStart hook
Session: [GL/COMMAND | BRAIN OPS | Brain Catchup · RAP-B Pre-Scan Hook | 260428]
- Full brain catchup 260413→260428: 7 files replaced (commit 88083e7) — command/{state,decisions,patterns,killed,research}.md + gl/{principles,decisions}.md
- Public mirror sync verified live; sync-public.yml workflow inspected (blocklist rsync approach, gl/entities.md excluded — correct)
- Rule 10 root cause diagnosed (Opus): RAP-B header is emitted before tool calls can fire, so declaration precedes scan. Sequencing bug, not knowledge gap. Documentation fixes don't survive — format must change.
- Structural fix shipped: SessionStart hook ~/.claude/hooks/rap-b-prescan.js auto-injects 156-agent categorized inventory via hookSpecificOutput.additionalContext at session start
- settings.json updated: new SessionStart hook + existing PreToolUse (git preflight) and UserPromptSubmit (oracle-preflight stub) preserved
- feedback_rap_enforcement.md updated with sequencing-bug root cause + new application rule
- ACTIVATION: requires `/hooks` reload OR fresh session — current session can't fire SessionStart
- VERIFICATION TEST: in next session ask "what agents are available?" — if categorized list recited without running ls, hook works; if scan needed, escalate to UserPromptSubmit (populate oracle-preflight.sh stub)

### 260428 — Layer 2 Brain Ops deployed
Session: [GL/COMMAND | OPS | Layer 2 Deploy · Brevo Alert · Heartbeat Cron | 260428]
- D1: POINTER_COMMAND.md v2 (Freshness Gate) uploaded to claude.ai COMMAND project knowledge — old v1 deleted, v2 (105 lines) confirmed persistent via page reload. DataTransfer JS injection used (Chrome ext file_upload blocked on hidden input).
- D2: sync-public.yml "Notify on failure" Brevo alert step confirmed present + pushed (commit 0a5fb4e). Sync run 25036215409 passed in 15s on push.
- D3: brain-heartbeat.yml cron corrected 13:00 → 12:00 UTC (EDT → BRT, São Paulo steady-state). Manual trigger run 25036290439 passed in 9s — log: "Last updated: 260428 (5 hours ago)". Brain fresh, no alert fired. Correct behavior.
- BREVO_API_KEY: now added to globalink-brain repo secrets (push succeeded). Both workflows live.
- brain-committer gap: .github/workflows/ out of append-only scope — committed directly per spec.

### 260428 — Layer 1 Brain Freshness Gate deployed
Session: [GL/COMMAND | BRAIN OPS | Layer 1 Freshness Gate · POINTER_COMMAND v2 | 260428]
- POINTER_COMMAND.md overwritten with v2 (84 ins / 19 del)
- Freshness Gate: mandatory session-start check — fetches state.md with cache-bust, parses Last updated, shows 🟡/🔴 banner if stale
- Routed through brain-committer agent (commit 5132856)
- Public mirror verified live — gate section confirmed at head of file

## Active Fixes (from audit 260413 — still open)
- Critical: "Run in [Agent]" button dead (StepDetailSidebar:551)
- Critical: ROI tracker inflated baseline
- Critical: Smart suggestions wrong fallback
- High: Two auto-handoff implementations (deduplicate lib/pipeline/)
- High: No vendor fetch timeout (30s AbortController per patterns.md)
- High: API keys written client-side (must route server-side)

## Pending Manual Steps

### Vercel Env Vars Needed:
STRIPE_FM_PRICE_ID
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
GOOGLE_REDIRECT_URI
HUBSPOT_CLIENT_ID
HUBSPOT_CLIENT_SECRET
HUBSPOT_REDIRECT_URI

### Third-Party Setup Needed:
- Google Cloud Console: OAuth 2.0 app, 3 scopes, redirect URI
- HubSpot Developers: Public app, 5 scopes, redirect URI

### GitHub Actions — Fix NLT Jun 1:
- Sync Action uses actions/checkout@v4 on Node.js 20
- Node.js 20 deprecated June 2 2026
- Add on-failure email alert to sync workflow (Brevo)
- Fix before retirement date

## Brain Infrastructure
Public mirror (globalink-brain-public) was archived 260422.
Caused 6 days of silent 403 failures on sync Action.
Unarchived 260428 — mirror now current.

## GTM Pipeline
### Eric — P1 priority
Beta invite window: May 5–10
Must never see an API key screen
Phase 2 pooled keys power all proxy calls
BYOK opt-in via Settings > Integrations only

### Grant Carlson (Global Entry Hub) — T1 warm
Discovery call done
One-pager sent
Follow-up sent 260420 (soft check-in: reaction + referral ask)
No beta invite yet — awaiting reply

### Jon Smith (Veteran Horizons) — T1 warm
Outreach active

### Richard Squires — T1 (Blaz referral)
Call done 260422
Email: richardsquires1@hotmail.com
Sgt Maj McClaflin intro pending

### Waalaxy Advanced (~€49/mo active)
T1 campaign: 38 contacts
T2 campaign: 193 contacts

## FM Cohort
25 slots | $99/mo | Closes Sep 30 2026
0 slots filled | Beta free until ~Aug 1
Nothing charges until beta ends
GTM primary motion: LinkedIn community-first (3x/week)
Content prep window: Apr 20-30 (BEFORE SP arrival)
Auto-publish window: May 1 - Jun 1
Eric beta hard gate: triage Items 1-6 by May 3

## Gated to First Revenue
Trademark COMMAND USPTO Class 42 (~$350)
Vouch insurance Cyber+E&O $155.75/mo (get Embroker quote first)
Axiom log drain (dataset: command-prod)
Provisional patent AEP+CCF+MACP
YC app (5 Phase 3 conditions)
Navattic rebuild (after beta testing complete)
ORACLE integration into COMMAND
