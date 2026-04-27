---
name: init-project-lite
description: Use when starting a small or medium project that needs end-to-end documentation without the full 22-file tree of init-project-docs. Triggers on phrases like "lightweight init", "minimal docs", "quick project setup", "scaffold docs lite", "bootstrap docs", or when the user asks for project docs but the scope is small enough that one folder beats seven.
---

# Init Project Docs (Lite)

## Overview

Same discipline as `init-project-docs`, smaller surface. Every project still needs the same conceptual coverage — what it does, how it's built, how to run it, how it ships, what's non-negotiable — but most projects don't need 22 files to express it. This skill folds that coverage into **5 files**: a `README.md`, three `docs/*.md` files grouped by audience, and `CLAUDE.md`. Same interrogate-then-generate-then-verify loop. Fewer stubs.

## When to Use

- Small or medium projects (single service, MVP, side project, internal tool)
- A team of 1–5 needs onboarding docs without a 7-folder maze
- The full `init-project-docs` tree would create more sparse stubs than useful pages
- User says "init lite", "minimal docs", "quick scaffold", or asks for project docs in a small repo

**Do NOT use for:**
- Large multi-service systems with multiple compliance frameworks → use `init-project-docs`
- Projects with 5+ external integrations or multiple data domains → use `init-project-docs`
- Adding a single doc to an already-documented project (just write the doc)

---

## Phase 1 — Interrogate First

**Never generate docs without answers.** Ask all five questions in a single message and wait.

```
1. Pitch + users + business model:
   - One-sentence elevator pitch
   - End users
   - B2C / B2B / B2B2C / internal
   - Monetization model (if any)

2. Stack + package manager:
   - Frontend, backend, database, auth, storage, hosting
   - The single canonical package manager (pnpm, npm, yarn, cargo, uv, etc.)

3. Data model + external integrations:
   - Key entities and their relationships at a high level
   - Every third-party API or service the system touches

4. Compliance + deployment:
   - Compliance obligations (HIPAA, SOC 2, GDPR, PCI, none)
   - Deployment target (cloud, region, container vs serverless, CI/CD)

5. Pre-made non-negotiable decisions:
   - Architectural rules, library choices, or invariants that are already locked in
     and must appear in CLAUDE.md
```

Wait for answers before Phase 2.

---

## Phase 2 — Generate the Doc Suite

Create exactly these five files. Adapt section depth to the answers — skip subsections that genuinely don't apply, but never omit a file.

### Directory Layout

```
/
  README.md                # human landing page
  docs/
    overview.md            # what / why / users / scope
    architecture.md        # stack / system / data / integrations / ops
    development.md         # setup / env / standards / testing / security
  CLAUDE.md                # AI navigation guide
```

### `README.md`
- Project name + one-sentence pitch
- Quickstart: clone → install → run, using the user's actual package manager
- Link table to the three `docs/*.md` files and `CLAUDE.md`
- License line (if known)

### `docs/overview.md`
- Problem statement
- Solution summary
- Target users
- Business model (if any)
- Non-goals — explicit scope boundaries
- Success criteria — what "done" looks like for v1

### `docs/architecture.md`
- **Tech stack table** — Layer | Technology | Why chosen
- **Component diagram** — ASCII or Mermaid; show user request → frontend → backend → data store → back
- **Schema section** — every entity/table: fields, types, indexes, required vs optional. This is the canonical source of truth — code matches the doc, not vice versa.
- **Integrations section** — one subsection per third-party API: purpose, auth model, rate limits, failure mode. Skip the section entirely if there are none.
- **Operations section** — infrastructure (cloud resources, IaC), deployment pipeline (CI/CD steps, secrets policy), monitoring (observability stack, alert routing). Brief — one paragraph per topic is fine.

### `docs/development.md`
- **Local setup** — zero-to-running, step by step. A new engineer should be able to follow this alone.
- **Environment variables** — table: name | required/optional | source | what it controls. Document shape, never values.
- **Coding standards** — language rules, naming conventions, required patterns, banned anti-patterns, code review checklist (copy-pasteable checkboxes)
- **Git workflow** — branch naming, commit message style, PR checklist
- **Testing strategy** — unit / integration / E2E tools and coverage targets
- **Security** — threat model summary, compliance obligations stated explicitly. Include PHI handling subsection only if health data is involved. Skip the whole section only if compliance answer was "none" AND there is no user data at all.

### `CLAUDE.md` — AI Navigation Guide

The most-read file in an AI-assisted project. Required structure:

```markdown
# [Project Name] — Claude Navigation Guide

[2-sentence project description]

---

## Package manager
[pnpm / npm / yarn / cargo / uv / etc.]

---

## Tech stack (quick reference)
| Layer | Technology |
|---|---|
| Frontend | ... |
| Backend | ... |
| Database | ... |
...

---

## Architecture
[ASCII or Mermaid diagram of the system]

---

## Non-negotiable rules
[Numbered list — architecture invariants, security boundaries, compliance
requirements. At least 5 rules. Every rule must be enforceable, not aspirational.]

---

## Documentation map
| If you need to know... | Read... |
|---|---|
| Why this project exists | docs/overview.md |
| How the system is built | docs/architecture.md |
| How to run / contribute | docs/development.md |
| ...                     | ...                |

---

## Key files
| Path | Purpose |
|---|---|
| ... | ... |
```

---

## Phase 3 — Verify Completeness

Before declaring docs complete, every box must be checked:

- [ ] A new engineer can clone, install, and run the project from `docs/development.md` alone
- [ ] Every env var the codebase reads is in the `docs/development.md` env var table
- [ ] Every entity referenced in code has a row in the `docs/architecture.md` schema section
- [ ] Every external integration named in Phase 1 has its own subsection in `docs/architecture.md`
- [ ] `CLAUDE.md` lists at least 5 numbered non-negotiable rules
- [ ] `CLAUDE.md` documentation map points to every file under `docs/`
- [ ] If compliance applies (Phase 1 answer ≠ "none"), `docs/development.md` has an explicit security section naming the obligations — not vague language like "we take security seriously"
- [ ] All Mermaid diagrams parse — simple node labels, no special characters in IDs
- [ ] `README.md` quickstart was mentally walked through against a fresh checkout — every command listed is actually correct

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Generating docs before asking the 5 questions | Always interrogate first — assumptions produce useless docs |
| Skipping `CLAUDE.md` because "the README is enough" | `CLAUDE.md` is the most-read file in AI-assisted work — never skip it, never collapse it into README |
| Folding everything into `README.md` | `README.md` is for newcomers, not for code-review rules or schema. Keep the 4-way split. |
| Vague non-negotiable rules ("write clean code") | Rules must be enforceable. "No `any` in TypeScript" is a rule. "Be a good citizen" is not. |
| Putting real secrets in the env vars table | Document name, required/optional, source, and what it controls. Never values. |
| Schema section that drifts from code | State in `CLAUDE.md` that the schema doc is canonical — code follows the doc |
| Using this skill on a multi-service compliance-heavy project | Switch to `init-project-docs` — the 5-file footprint will paper over real complexity |
