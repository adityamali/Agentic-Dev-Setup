---
id: ADR-0005
title: Skills coexist with lock-managed skill packages
status: accepted
date: 2026-08-14
related: [ADR-0002]
---

# ADR-0005: Skills coexist with lock-managed skill packages

## Context

`~/.agents/skills/` already contained 11 Cloudflare skills installed by a skill CLI using `.skill-lock.json`. The user asked for a complete skill management framework but explicitly forbade populating it with real engineering skills.

## Problem

How do we add a skill framework and allow locally authored skills without conflicting with an existing lock-managed package directory?

## Decision

- Treat `skills/` as a **package repository** that supports two kinds of skills:
  1. **Lock-managed**: installed by a skill CLI; content tracked by `.skill-lock.json`, content ignored by git.
  2. **Locally authored**: created directly in this repo; tracked by git.
- Add framework files (`README.md`, `SPEC.md`) and a template (`_template/`). The underscore prefix marks scaffolding that installers must skip.
- Keep `.skill-lock.json` tracked in git as the reproducible record of installed vendored skills.
- Ignore the content of lock-managed skill folders in `.gitignore`; add each new vendored install to `.gitignore`.

## Alternatives considered

1. **Move existing skills out and rebuild `skills/` from scratch.**
   - Rejected: the existing skills are already used by tools; moving them would break tool wiring.
2. **Track all skill content in git.**
   - Rejected: would bloat history and create merge conflicts when the skill CLI updates vendored packages.
3. **Hybrid lock-managed + local, lock file tracked, vendored content ignored — chosen.**

## Consequences

### Positive
- Existing skills continue to work untouched.
- New local skills are first-class git content.
- Reproducible installs via `.skill-lock.json`.

### Negative
- `.gitignore` must be updated when a new vendored skill is installed.
- Agents must not edit lock-managed skills.

## References

- `../skills/README.md`
- `../skills/SPEC.md`
- `../system/registry.md`
- `../system/tool-wiring.md`
