# COMMAND — Decisions Register
Last updated: 260413

## 260413 — Skills Enforcement KEPT
Decision: Rejected Gemini kill recommendation
Rationale: Trust layer not governance layer. Sandra never
  sees compliance language. Rebranded "Agent Guardrails."
  Without enforcement, prompt injection destroys trust.
Alternative rejected: Kill entirely

## 260413 — Semantic Matchmaking KEPT
Decision: Rejected Gemini kill recommendation
Rationale: Required for non-technical UX. Without it Sandra
  must manually select agents — defeats the product promise.
Alternative rejected: Remove in favor of deterministic routing

## 260413 — Workspace DNA over Mem0/Zep
Decision: Simple text field, system prompt injection
Rationale: Zero infrastructure. 80% of perceived second brain
  value at $0 cost. Mem0 graph features = $249/mo. Wrong tier.
Revisit: Phase 3 — Zep temporal graph if Sandra needs
  entity tracking across sessions

## 260413 — Google OAuth before HubSpot
Decision: Google Workspace shipped first
Rationale: Sandra's confirmed stack is Google-native.
  ChatGPT + Claude + Perplexity + Fathom + Notion + Gmail.
  HubSpot penetration lower than assumed in boutique segment.

## 260413 — Canvas: linear execution only
Decision: 15-25hr linear build, not 150-200hr full engine
Rationale: Non-linear branching, async, loop prevention is
  Phase 4. Sandra runs Research → Draft → Review. Linear only.

## 260413 — ROI Tracker over Agent DNA Editor as Phase 2.9
Decision: ROI Tracker is Phase 2.9 priority
Rationale: Renewal conversation framing built into product.
  "COMMAND saved you 14 hours this week" closes the billing gap.

## 260413 — Phase/date rule PERMANENT
Decision: Builds framed by phase+function, never calendar
Rationale: Jason completes "week-long" items in hours.
  Time estimates are suggestions only. Train to standard.

## 260413 — Stripe price ID canonical names locked
Decision: One env var name per price ID. No aliases.
  STRIPE_FM_PRICE_ID, STRIPE_PRO_PRICE_ID,
  STRIPE_SOLO_PRICE_ID, STRIPE_PRICE_STUDIO,
  STRIPE_PRICE_AGENCY
Rationale: Legacy aliases removed in c30ad1a.
  Dual names caused silent failures.

## 260413 — FM cap race condition: Option B
Decision: Document known race, manual check after purchase.
Rationale: 25-seat cap makes simultaneous purchase
  probability near-zero. Redis lock is over-engineering
  for this volume. Review if cohort fills fast.

## 260413 — system_prompt redacted from DB
Decision: Store hash + length, not full prompt.
Rationale: Workspace DNA + API patterns in plaintext
  in task_executions was a security gap. Hash preserves
  auditability without storing sensitive content.

## 260413 — Gemini vendor status: pending not null
Decision: Gemini/cursor/custom vendors return pending
  status rather than null. UI shows "coming soon."
Rationale: Returning null silently made Sandra think
  her Gemini agent was working when it wasn't.
