# GlobaLink — Decisions Register
Last updated: 260428

## Decisions affecting all entities logged here.
Project-specific decisions are in their respective decisions.md files.

---

## 260427 — Enterprise Doctrine v1.2 adopted

Decision: ORACLE-KB Enterprise Doctrine v1.2 is canonical
  operating law across all Claude variants.

Rules added in v1.2:
  Rule 9 — Action Officer Doctrine: route directives,
    Jason reviews+approves, never hands Jason todo lists.
  Rule 10 — RAP-B Real-Scan: every non-trivial task scans
    /mnt/skills/public + ~/.claude/agents + .cursor/rules +
    RPM packs. Stamping without scanning = Rule 10 violation.
  Rule 11 — Memory-Check-Precedes-Recon: surface memory
    before filesystem recon on paths/repo state/auth/installs.

Prior rules (v1.0–v1.1): atomic closeout, variant selection,
  skill gating, no preview-then-execute drift, voice+format,
  entity firewall, spec freshness check.

---

## 260427 — ORACLE-KB-SPEC v1.2 adopted

Decision: ORACLE-KB-SPEC v1.2 is the canonical knowledge OS
  spec across all Claude projects.

Breaking change: Chat naming convention patched.
  Leading namespace changed from PROJECT to GL universally.
  Multi-topic separator: · (U+00B7 middle-dot).
  Start-date semantics: immutable, infer from context.
  Anti-patterns added for the three naming failure modes.

Bootstrap kit updated to v1.2 — refresh in all 12+ projects.

---

## 260427 — Agent/skill library audit formalized

Decision: Library audit runs first Monday monthly via Cowork.
Sources: awesome-claude-code, awesome-mcp-servers,
  Anthropic docs/blog, NPM @anthropic-ai, r/ClaudeAI.
Vetting gate: 500+ stars or Anthropic-affiliated,
  ≤90d last commit, permissive license, no telemetry.
Current state (260427):
  ~/.claude/agents/ — 190 .md files
    (189 STOCK upstream + 1 ORIGINAL: cc-prompt-architect)
  .cursor/rules — 162 .mdc all stock
  9 RPM plugin packs ~100 skills
ORIGINAL agent (cc-prompt-architect) is COMMAND-specific,
  local only — never upstream.

---

## 260413 — GL Build OS v1.1 adopted as canonical GL build methodology

Decision: gl/build-os.md (v1.1) is the canonical zero-to-one
  build methodology for all GL products. Any new product —
  Ponte, future GL entities — runs through its 8 sections
  before committing to code.

How v1 was produced: Extracted from the COMMAND build corpus
  in an Opus session (Pass 1 of 260413 session). 8 sections:
  Phase Architecture, ICP Surgery, Positioning/Moat, Novel
  Concept Research, Psychology Layer, AI Tool Stack,
  GTM Architecture, Defensibility Protection.

How v1.1 was produced: Adversarial Ponte stress test
  (Pass 2 of same session). Six failure findings surfaced;
  six patches applied:

1. §1 — Bilateral validation rule + Phase 0.5 Concierge
2. §2 — 1:1 ratio rule for bilateral ICPs
3. §3 — Moat Type Taxonomy (8 types, not software-only)
4. §5 — Cultural context override on cognitive-load rules
5. §6 — Operator Fieldwork as first-class lane type
6. §8 — Renamed "Defensibility Protection," four branches
   (patent + trademark + trade secret + regulatory)

Downstream:
  Ponte adopts v1.1 as its build methodology
  Phase 0.5 Concierge mandatory for Ponte
  Ponte moat stack is non-software

---

## 260413 — Claude Code Tooling Standards

Decision: Implement all /insights recommendations as hard
  rules across all repos.

What changed:
- Pre-commit hook added globally: blocks commits with
  TypeScript or ESLint errors
- /preflight and /closeout custom commands created
- GIT DISCIPLINE rule is now universal:
  never git add . or git add -A — always specific files
- SQL DISCIPLINE added to command-app:
  always query information_schema before writing migrations
- SESSION PROTOCOL and BRAIN REPO rules now in root
  CLAUDE.md and repo-level CLAUDE.md files
- SECURITY rule formalized:
  never suggest pasting secrets in chat

Why: 177 sessions analyzed — top friction sources were buggy
  code requiring multi-round fixes, Claude not reading context
  before acting, and git staging collisions from parallel
  agent sessions.
