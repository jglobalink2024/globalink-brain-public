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

## Active Fixes (from audit 260413)
- Critical: "Run in [Agent]" button dead (StepDetailSidebar:551)
- Critical: ROI tracker inflated baseline
- Critical: Smart suggestions wrong fallback
- High: Two auto-handoff implementations (deduplicate)
- High: No vendor fetch timeout
- High: MCP endpoint_url column missing
- High: MCP plan caps inconsistent
- High: API keys written client-side

## Pending Manual Steps
### Supabase SQL Editor (apply in order):
All 15 April 13 migrations — see R5 audit for full list
Plus: MCP endpoint_url migration (from Fix 6 above)

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

## FM Cohort
25 slots | $99/mo | Closes Sep 30 2026
0 slots filled | Beta free until ~Aug 1
