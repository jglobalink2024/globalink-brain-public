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
