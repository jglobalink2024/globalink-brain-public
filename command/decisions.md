# COMMAND — Decisions Register
Last updated: 260428

## 260428 — Public mirror archived root cause resolved
Decision: globalink-brain-public was archived 260422, causing
  6 days of silent 403 failures on sync Action. Unarchived 260428.
Action: Add on-failure email alert to sync workflow (Brevo).
  Upgrade Node.js 20 in Action before Jun 2 deprecation deadline.
Revive condition: N/A — operational fix

## 260427 — HOT MEMORY DOCTRINE
Decision: Hot memory stores invariant behavioral rules ONLY.
Kill test: "Would Claude do the wrong thing before reading even
  one file?" Yes → hot memory. No → brain.
Brain routing:
  state.md → build/GTM/bugs
  patterns.md → repo/build conventions
  decisions.md → architecture/IP
  killed.md → rejected ideas
  research.md → competitive/market
  entities.md → contacts (private, never synced)
Rationale: State, contacts, intel, and reference data in hot
  memory bloats context and creates staleness risk.

## 260427 — ACTION OFFICER DOCTRINE (Rule 9)
Decision: When Jason gives a digital directive, Claude routes
  to correct variant and executes. Jason reviews and approves.
Acceptable for Jason as actor only:
  physical-world tasks, irreversible legal/financial signatures,
  identity-required tasks.
Never default to handing Jason a todo list.
Violation signal: "stop making me the action officer" →
  self-correct immediately.

## 260427 — RAP-B REAL-SCAN RULE (Rule 10)
Decision: Every non-trivial task requires real scan of:
  /mnt/skills/public + ~/.claude/agents (190 files) +
  .cursor/rules (162 .mdc) + 9 RPM plugin packs.
RAP-B "none" must always be qualified by what was checked.
Stamping without scanning is a Rule 10 violation.

## 260427 — MEMORY-CHECK-PRECEDES-RECON RULE (Rule 11)
Decision: Before filesystem recon on any task involving paths,
  repo state, auth, or install locations — surface relevant
  memory first. Discovering through recon what memory already
  encoded is a Rule 11 violation.

## 260427 — AGENT/SKILL LIBRARY AUDIT CADENCE
Decision: Cowork scans first Monday monthly.
Sources: awesome-claude-code, awesome-mcp-servers,
  Anthropic docs/blog, NPM @anthropic-ai, r/ClaudeAI.
Vetting: 500+ stars or Anthropic-affiliated, ≤90d commit,
  permissive license, no telemetry.
Current inventory: ~/.claude/agents/ 190 .md files
  (189 STOCK + 1 ORIGINAL: cc-prompt-architect, COMMAND-only).
  .cursor/rules 162 .mdc all stock. 9 RPM plugin packs ~100 skills.

## 260416 — API key security: pooled keys default
Decision: Onboarding is API-key-free by default.
  COMMAND pooled keys power all proxy calls.
  BYOK opt-in only via Settings > Integrations.
Enforcement: Eric must never see an API key screen.
Rationale: "Another tool to manage" is the primary kill trigger.
  Key friction must be zero at first contact.

## 260416 — MCP scope overlap warning: Phase 3
Decision: Scope field on MCP status endpoint queued for Phase 3.
  Warns when two registered MCPs have overlapping capability sets.
Gate: first paying customer (same as Phase 3).

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

## 260418 — GTM primary motion: Community-first, not outreach-first
Decision: LinkedIn community-first replaces Waalaxy as primary
  top-of-funnel. Waalaxy demoted to research instrument (€49/mo).
Rationale: Cold outreach yields 3-5 conversations/6wks — research
  instrument, not growth engine. Community-led is fastest
  rapid-growth lever for bootstrap solo founder.
Cancel trigger: End of June; if zero discovery calls + zero
  usable buyer-language quotes harvested, cancel Waalaxy.

## 260418 — Pricing model: Flat + caps + overages (hybrid)
Decision: Reject pure usage. Keep flat as primary. Add task caps
  per tier with overage upsell to next tier. Introduce $29
  Starter tier. Preserve $99 FM locked-rate cohort.
Rationale: Sandra's mental model is flat (ChatGPT/Claude/Perplexity
  all flat). Pure usage breaks conversion, kills engagement,
  destroys FM offer, complicates forecasting.
Revisit: Q3 2026 — credit packs if 3+ FM customers ask for
  pay-as-you-go in discovery.
