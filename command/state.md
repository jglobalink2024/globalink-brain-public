# COMMAND — Current State
Last updated: 260413

## Live URLs
App: app.command.globalinkservices.io
Landing: command.globalinkservices.io
Sales Hub: command.globalinkservices.io/sales-hub
Beta Syllabus: command.globalinkservices.io/beta-syllabus
NDA: go.command.globalinkservices.io/betaNDA
Stripe FM: https://buy.stripe.com/9B68wRgwAcp8dt2dJp9k407
Stripe Pro: https://buy.stripe.com/bJe00lfsw74O74E5cT9k408

## Build State
Phase 1 + 1.5: COMPLETE
Phase 2: COMPLETE (all 15 sub-phases shipped 260413)
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

## Fixes Shipped (260413)
Wave 1 (bcaa350) — 8 critical fixes:
- Canvas "Run in [Agent]" button — FIXED
- ROI adaptive baseline + credibility cap — FIXED
- Smart suggestions null fallback — FIXED
- Auto-handoff deduplicated — FIXED
- Vendor fetch 30s timeout — FIXED
- MCP endpoint_url migration — FIXED
- MCP plan caps matched to planGate — FIXED
- API keys through server route — FIXED

Wave 2 (Session U) — 14 high fixes: IN FLIGHT
Wave 3 (Session V) — full polish pass: IN FLIGHT

## Next Session Priorities
1. Confirm Session U + V landed clean
2. Upload updated POINTER files to Claude.ai projects
3. Apply pending Supabase migrations
4. Add Vercel env vars (Google + HubSpot + Stripe FM)
5. Send Eric beta invite
6. Symphony v9 — waiting on spec v1

## Pending Manual Steps
### Supabase SQL Editor (apply in order):
20260413120001_phase2_3_task_chain.sql
20260413120002_phase2_5_skills_enforcement.sql
20260413000000_canvas_task_bridge.sql
20260413210000_workspace_dna.sql
20260413300000_stripe_billing.sql
20260413400000_semantic_matchmaking.sql
20260413500000_google_oauth.sql
20260413600000_canvas_templates.sql
20260413600001_roi_tracker.sql
20260413700000_hubspot_oauth.sql
20260413220000_mcp_endpoint_url.sql

### Vercel Env Vars Needed:
STRIPE_FM_PRICE_ID
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
GOOGLE_REDIRECT_URI=https://app.command.globalinkservices.io/api/integrations/google/callback
HUBSPOT_CLIENT_ID
HUBSPOT_CLIENT_SECRET
HUBSPOT_REDIRECT_URI=https://app.command.globalinkservices.io/api/integrations/hubspot/callback

### Third-Party Setup Needed:
- Google Cloud Console: OAuth 2.0 client, 3 scopes
- HubSpot Developers: Public app, 5 scopes

## FM Cohort
25 slots | $99/mo | Closes Sep 30 2026
0 slots filled | Beta free until ~Aug 1
