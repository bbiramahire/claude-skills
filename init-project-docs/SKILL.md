---
name: init-project-docs
description: Use when starting a new project or initializing documentation for an existing project before development begins. Triggers on phrases like "initialize project", "set up docs", "create project documentation", "start a new project", or before any first implementation task on a project with no existing docs.
---

# Init Project Docs

## Overview

Before writing a single line of implementation code, every project needs a structured documentation suite. This skill generates the full doc tree by asking the right questions first, then producing targeted documentation files. The goal is one complete, coherent reference that survives the project's lifetime.

## When to Use

- Starting a new project from scratch
- Inheriting a project with no docs
- Beginning a major new feature domain with its own architecture
- User says "initialize", "set up docs", "scaffold the project", or "document the project"

**Do NOT use for:**
- Adding a single doc file to an already-documented project
- Writing feature specs (use the feature-spec workflow instead)

---

## Phase 1 — Interrogate First

**Never generate docs without answers to these questions.** Ask them all at once in a single message.

```
1. What does this project do? (one sentence — the elevator pitch)
2. Who are the end users and what is the business model (B2C, B2B, B2B2C, internal tool)?
3. What is the tech stack? (frontend framework, backend, database, auth, storage, cloud)
4. Are there any compliance or security requirements? (HIPAA, SOC 2, GDPR, PCI, none)
5. What external integrations or third-party APIs are involved?
6. What is the data model at a high level? (key entities and their relationships)
7. What are the deployment targets? (cloud provider, region, container vs serverless)
8. What is the team size and skill profile?
9. What is the primary package manager / language toolchain?
10. Are there any non-negotiable architectural decisions already made?
```

Wait for answers before proceeding to Phase 2.

---

## Phase 2 — Generate the Doc Suite

Create the following files. Adapt section depth to what was answered — skip sections that genuinely don't apply, but never omit a file entirely.

### Directory Layout

```
docs/
  project.md                    # What & why
  summary.md                    # 1-pager for new teammates
  architecture.md               # System design + component diagram
  tech-stack.md                 # Every library/service, why it was chosen
  data/
    overview.md                 # Data layer rationale
    schema.md                   # Canonical entity/table definitions
    entity-relationships.md     # ERD in Mermaid
  api/
    overview.md                 # Full API surface map
    [integration].md            # One file per external integration
  development/
    local-setup.md              # Zero to running — step by step
    environment-variables.md    # Every env var, required/optional, source
    coding-standards.md         # Language rules, naming, patterns, anti-patterns
    git-workflow.md             # Branch naming, commit style, PR checklist
    testing-strategy.md         # Unit, integration, E2E — tools + coverage targets
  security/
    overview.md                 # Threat model summary, compliance obligations
    [compliance].md             # One file per compliance framework if applicable
    phi-handling.md             # Only if health data is involved
  operations/
    infrastructure.md           # Cloud resources, IaC, cost estimates
    deployment-pipeline.md      # CI/CD steps, secrets policy
    monitoring.md               # Observability stack, alert routing
  plans/
    adr-template.md             # Blank ADR for future decisions
    feature-spec-template.md    # Blank feature spec
CLAUDE.md                       # AI navigation guide (see structure below)
```

---

## File Content Standards

### `docs/project.md`
- Problem statement
- Solution summary
- Target users
- Business model and pricing (if known)
- Non-goals (explicit scope boundaries)

### `docs/summary.md`
- One-page overview: what it does, who uses it, how money is made
- Key architectural decisions in bullet form
- Links to the 3 most important docs for a new engineer

### `docs/architecture.md`
- Component diagram (ASCII or Mermaid)
- Data flow: user request → frontend → backend → data store → back
- Key design decisions with brief rationale
- What is intentionally NOT in scope

### `docs/data/schema.md`
- Every entity/table: fields, types, indexes, required vs optional
- Written to be the single source of truth — code must match this, not vice versa

### `docs/development/coding-standards.md`
- Language-specific strict rules (no `any`, no unnamed magic numbers, etc.)
- Naming conventions table
- Patterns that are required (e.g., mutations in hooks, DTOs on controllers)
- Anti-patterns that are banned with reason
- Code review checklist (checkbox list, copy-pasteable)

### `docs/development/environment-variables.md`
- Table: variable name | required/optional | source | what it controls
- Never include actual values — document shape, not secrets

### `CLAUDE.md` — AI Navigation Guide

This is the most critical file. It must contain:

```markdown
# [Project Name] — Claude Navigation Guide

[2-sentence project description]

---

## Package manager
[pnpm / npm / yarn / cargo / etc.]

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
[Numbered list of rules that must never be violated — architecture invariants, security boundaries, compliance requirements]

---

## Documentation map
[Table: question → which doc file answers it]

---

## Key files
[Table: file path → purpose]
```

---

## Phase 3 — Verify Completeness

Before declaring docs complete, check every item:

- [ ] A new engineer could run the project locally using only `local-setup.md`
- [ ] Every env var in the codebase is listed in `environment-variables.md`
- [ ] Every entity in the schema has a corresponding entry in `data/schema.md`
- [ ] Every external integration has its own doc in `docs/api/`
- [ ] `CLAUDE.md` names at least 5 non-negotiable architectural rules
- [ ] `coding-standards.md` has a code review checklist
- [ ] Security/compliance obligations are stated explicitly, not vaguely
- [ ] All Mermaid diagrams are syntactically valid (use simple node labels, avoid special chars)
- [ ] `docs/plans/feature-spec-template.md` includes a PHI impact section if health data is present

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Generating docs before asking questions | Always interrogate first — assumptions produce useless docs |
| Writing vague architecture docs ("uses microservices") | Be concrete — name services, show data flows, include actual port numbers |
| Skipping `CLAUDE.md` | This is the most-read file in an AI-assisted project — never skip it |
| Putting real secrets in env var docs | Document shape and source, never actual values |
| One giant `README.md` instead of a doc suite | A monolith README rots fast; a categorized doc suite stays navigable |
| Schema docs that drift from code | Establish in `CLAUDE.md` that the doc is canonical — code follows the doc |
| Missing non-negotiable rules in `CLAUDE.md` | Rules must be explicit and numbered, not implied |
