# claude-skills

A small collection of opinionated [Claude Code](https://claude.com/claude-code) skills.

| Skill | Use it when |
|---|---|
| [`init-project-docs`](./init-project-docs) | Starting a real, multi-component project that needs a full documentation suite — architecture, data, API, security, operations, AI navigation. Generates a 22-file doc tree across 7 folders. |
| [`init-project-lite`](./init-project-lite) | Starting a small or medium project — single service, MVP, side project, internal tool. Same conceptual coverage as `init-project-docs`, but folded into 5 files instead of 22. |
| [`repo-malware-sweep`](./repo-malware-sweep) | A developer/org may be hit by a PolinRider-class supply-chain compromise — an obfuscated loader injected into build config files, fake fonts, or `.vscode` auto-run tasks and pushed across every repo a stolen credential can reach. Runs containment-first IR: rotate creds, sweep every org on fresh origin, report per-owner, remediate with reviewable diffs. |

The two `init-project-*` skills share one discipline: **interrogate first, generate second, verify last**. `repo-malware-sweep` is a security/incident-response skill with its own **containment-first** discipline: **rotate credentials before you scan, scan fresh origin, remediate only with authorization.**

---

## Install

Copy whichever skills you want into your personal skills folder:

```bash
git clone https://github.com/bbiramahire/claude-skills.git
mkdir -p ~/.claude/skills
cp -r claude-skills/init-project-lite ~/.claude/skills/
cp -r claude-skills/init-project-docs ~/.claude/skills/
cp -r claude-skills/repo-malware-sweep ~/.claude/skills/
```

Or symlink them so you can `git pull` updates:

```bash
git clone https://github.com/bbiramahire/claude-skills.git ~/Developer/claude-skills
mkdir -p ~/.claude/skills
ln -s ~/Developer/claude-skills/init-project-lite ~/.claude/skills/init-project-lite
ln -s ~/Developer/claude-skills/init-project-docs ~/.claude/skills/init-project-docs
ln -s ~/Developer/claude-skills/repo-malware-sweep ~/.claude/skills/repo-malware-sweep
```

Restart Claude Code (or open a new session). The skills will appear in the available-skills list and can be invoked as `/init-project-docs` or `/init-project-lite`.

---

## How the two skills relate

Both skills run the same three-phase loop:

1. **Interrogate** — ask the user a fixed set of questions before generating anything. Assumptions produce useless docs.
2. **Generate** — produce the doc files based on the answers.
3. **Verify** — run a checklist before declaring docs complete.

They differ in:

| | `init-project-docs` | `init-project-lite` |
|---|---|---|
| Questions asked | 10 | 5 (grouped) |
| Files generated | ~22 across 7 folders | 5 (`README.md`, `docs/overview.md`, `docs/architecture.md`, `docs/development.md`, `CLAUDE.md`) |
| ADR + feature-spec templates | Yes | No |
| Conditional sections | Per-compliance-framework files, per-integration files | Same coverage, folded into existing sections |

If you start with `init-project-lite` and outgrow it, you can run `init-project-docs` later and split the existing files apart.

---

## What both skills always produce

A `CLAUDE.md` at the project root containing:

- A 2-sentence project description
- The canonical package manager
- A tech-stack quick-reference table
- An architecture diagram
- **Numbered non-negotiable rules** (architecture invariants, security boundaries, compliance requirements)
- A documentation map (question → doc file)
- A key-files table

This file is the most-read artifact in any AI-assisted project. Neither skill will skip it.

---

## License

[MIT](./LICENSE) — use, modify, redistribute freely.

---

## Contributing

Issues and PRs welcome. If you have a skill in the same spirit (rigorous, opinionated, focused on a single discipline), open a PR and we'll review it.
