# COMMAND — Current State
Last updated: 260428

## Live URLs
App: app.command.globalinkservices.io
Landing: command.globalinkservices.io
Sales Hub: command.globalinkservices.io/sales-hub
Beta Syllabus: command.globalinkservices.io/beta-syllabus
NDA: go.command.globalinkservices.io/betaNDA
Demo: go.command.globalinkservices.io/demo (Navattic)
Stripe FM: https://buy.stripe.com/9B68wRgwAcp8dt2dJp9k407
Stripe Pro: https://buy.stripe.com/bJe00lfsw74O74E5cT9k408

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

## Infra Audit 260416 — COMPLETE
40 migrations applied
7 env vars confirmed
NEXT_PUBLIC_GL_INTERNAL removed
Migration log updated
CLI injection reported to Vercel

## Session Log

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

### Supabase SQL Editor (apply after 260413 migrations):
- mcp_secret column on workspaces table
- mcp_endpoint_url column on agents table
- capabilities column on agents table

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

## Gated to First Revenue
Trademark COMMAND USPTO Class 42 (~$350)
Vouch insurance Cyber+E&O $155.75/mo (get Embroker quote first)
Axiom log drain (dataset: command-prod)
Provisional patent AEP+CCF+MACP
YC app (5 Phase 3 conditions)
Navattic rebuild (after beta testing complete)
ORACLE integration into COMMAND
