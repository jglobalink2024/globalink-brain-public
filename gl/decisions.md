# GlobaLink — Decisions Register
Last updated: 260413

## Decisions affecting all entities logged here.
Project-specific decisions are in their respective decisions.md files.

---

## 260413 — GL Build OS v1.1 adopted as canonical GL build methodology

**Decision:** `gl/build-os.md` (v1.1) is the canonical zero-to-one build methodology for all GL products. Any new product — Ponte, future GL entities — runs through its 8 sections before committing to code.

**How v1 was produced:** Extracted from the COMMAND build corpus in an Opus session (Pass 1 of 260413 session). 8 sections: Phase Architecture, ICP Surgery, Positioning/Moat, Novel Concept Research, Psychology Layer, AI Tool Stack, GTM Architecture, Defensibility Protection.

**How v1.1 was produced:** Adversarial Ponte stress test (Pass 2 of same session). Six failure findings surfaced; six patches applied:

1. **§1** — Bilateral validation rule + Phase 0.5 Concierge phase for marketplaces
2. **§2** — 1:1 ratio rule for bilateral ICPs
3. **§3** — Moat Type Taxonomy (8 types, not software-only)
4. **§5** — Cultural context override on cognitive-load rules (low-context vs high-context)
5. **§6** — Operator Fieldwork as a first-class lane type with handoff protocol
6. **§8** — Renamed "Defensibility Protection," four branches (patent + trademark + trade secret + regulatory), not patent-only

**Why:** v1 had implicit biases — single-sided, low-context, software-only — that were correct defaults for COMMAND and wrong defaults for Ponte and any non-COMMAND product. v1.1 is universally applicable.

**Companion artifacts:**
- `gl/build-os.md` — canonical v1.1 (the playbook)
- `gl/build-os-ponte-stress-test.md` — record of the adversarial application that produced v1.1

**Downstream decisions flowing from this:**
- Ponte adopts v1.1 as its build methodology (see `ponte/decisions.md`)
- Phase 0.5 Concierge becomes mandatory for Ponte
- Ponte moat stack is non-software (trademark + trade secret + regulatory + physical presence)

---

## 260413 — Claude Code Tooling Standards

**Decision:** Implement all /insights recommendations as hard rules across all repos.

**What changed:**
- Pre-commit hook added globally: blocks commits with TypeScript or ESLint errors
- /preflight and /closeout custom commands created
- GIT DISCIPLINE rule is now universal: never `git add .` or `git add -A` — always stage specific files
- SQL DISCIPLINE added to command-app: always query information_schema before writing migrations
- SESSION PROTOCOL and BRAIN REPO rules now in root CLAUDE.md and repo-level CLAUDE.md files
- SECURITY rule formalized: never suggest pasting secrets in chat

**Why:** 177 sessions analyzed — top friction sources were buggy code requiring multi-round fixes, Claude not reading context before acting, and git staging collisions from parallel agent sessions. These rules directly address each friction source.
