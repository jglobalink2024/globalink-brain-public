# COMMAND — Current State
Last updated: 260413

## Live URLs
App: app.command.globalinkservices.io
Landing: command.globalinkservices.io
NDA: go.command.globalinkservices.io/betaNDA
Stripe FM: https://buy.stripe.com/9B68wRgwAcp8dt2dJp9k407
Stripe Pro: https://buy.stripe.com/bJe00lfsw74O74E5cT9k408

## Build State
Phase 1 + 1.5: COMPLETE
Phase 2: COMPLETE — all 15 sub-phases + all audit
  fixes shipped. Product is audit-clean.
Phase 3: NOT STARTED (gate: first paying customer)

## Commits Today (260413)
a8e0412 Phase 2.1 real agent connections
0366c0f Phase 2.3 auto-handoff
ddd7364 Phase 2.4 router execution
b1969a9 Phase 2.6 canvas linear execution
f3ccfd8 Phase 2.7 semantic matchmaking
c20d16a Phase 2.X workspace DNA
2241bad Phase 2.4.5 smart suggestions
21076b1 Phase 2.5 skills + Google OAuth
3a1df14 Phase 2.Y RevOps templates
1a9cb40 Phase 2.9 ROI tracker
bcaa350 8 critical audit fixes (Wave 1)
c30ad1a 16 polish fixes (Wave 2 — Session V)
7400009 14 high fixes (Wave 3 — Session U)
0ef088d Brain POINTER files updated to public URLs
ab2583c Brain state update
87f8cec Brain initialization

## Audit Status — ALL CLOSED
Wave 1 (bcaa350): 8 critical — DONE
Wave 2 (c30ad1a): 16 polish — DONE
Wave 3 (7400009): 14 high — DONE
Total fixes: 38 across 3 sessions

## Pending Manual Steps
### Supabase SQL Editor — apply these:
20260413900000_task_executions_redact_prompt.sql
(+ all 15 April 13 migrations if not yet applied)

### Vercel Env Vars — verify canonical names:
STRIPE_FM_PRICE_ID (not STRIPE_PRICE_FM)
STRIPE_PRO_PRICE_ID (not STRIPE_PRICE_PRO)
STRIPE_SOLO_PRICE_ID — get from Stripe Dashboard
STRIPE_PRICE_STUDIO — create Studio product in Stripe
STRIPE_PRICE_AGENCY — create Agency product in Stripe
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
GOOGLE_REDIRECT_URI
HUBSPOT_CLIENT_ID
HUBSPOT_CLIENT_SECRET
HUBSPOT_REDIRECT_URI

### Third-Party Setup Still Needed:
- Google Cloud Console: OAuth 2.0 client + 3 scopes
- HubSpot Developers: Public app + 5 scopes

## Next Session Priorities
1. Symphony v9 results → fix any 🔴 CRITICAL findings
2. Send Eric beta invite (Phase 2 complete, ready now)
3. Grant Carlson 7-day follow-up (check date)
4. Apply pending Supabase migration above
5. Add missing Stripe price IDs to Vercel

## FM Cohort
25 slots | $99/mo | Closes Sep 30 2026
0 slots filled | Beta free until ~Aug 1 2026
