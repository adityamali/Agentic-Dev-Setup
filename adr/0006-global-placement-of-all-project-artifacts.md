---
id: ADR-0006
title: Global placement of all project artifacts
status: accepted
date: 2026-08-14
project: system
related: []
---

# ADR-0006: Global placement of all project artifacts

## Context

The user wants this repository to be the single long-term engineering brain for all projects on the machine. Project architecture docs, ADRs, and project notes could live either globally here or in each project's own repository.

## Problem

Where should project-scoped architecture docs, ADRs, and notes live so that every agent can consult them regardless of the current working directory?

## Decision

All artifacts live globally in `~/.agents/`:

- ADRs: flat namespace under `adr/`, tagged with a `project` frontmatter field. System ADRs omit the field.
- Architecture: `architecture/<project-slug>/` for each project.
- Project notes: `memories/projects/<project-slug>.md`.

Boundary:

- `architecture/<slug>/` holds the validated, decision-backed structural description.
- `memories/projects/<slug>.md` holds evolving observations, constraints, and running knowledge.

## Alternatives considered

1. **Hybrid: summaries here, full docs in project repos.**
   - Rejected: agents working in different directories would not reliably find project docs; the "single brain" goal is weakened.
2. **Everything in project repos, framework only here.**
   - Rejected: contradicts the explicit goal of a global, tool-agnostic engineering brain.
3. **Everything global — chosen.**

## Consequences

### Positive
- Every agent can access all knowledge regardless of `cwd`.
- Single index for ADRs and architecture.
- No risk of project repos omitting docs.

### Negative
- `~/.agents/` will grow with each project.
- `adr/INDEX.md` and `architecture/INDEX.md` require active maintenance.
- Project-specific ADRs are not in the project repo where some reviewers might expect them.

## References

- `../architecture/README.md`
- `../architecture/INDEX.md`
- `../memories/README.md`
- `../adr/README.md`
