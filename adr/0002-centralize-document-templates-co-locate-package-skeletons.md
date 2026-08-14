---
id: ADR-0002
title: Centralize document templates; co-locate package skeletons
status: accepted
date: 2026-08-14
related: [ADR-0001]
---

# ADR-0002: Centralize document templates; co-locate package skeletons

## Context

Agents will create ADRs, memory notes, architecture docs, feature specs, bug reports, and engineering proposals. They also need templates for structural packages such as skills and agents.

## Problem

Where should templates live so agents can find them quickly without duplication, while respecting that document templates and package skeletons are different shapes?

## Decision

- **Document templates** are centralized in `templates/` as the single source of truth for ADRs, memory notes, architecture docs, feature specs, bug reports, proposals, project summaries, and research notes.
- **Package skeletons** (`skills/_template/`, `agents/_template/`) are co-located with their framework documentation because they are directory structures with multiple optional files, not single documents.

## Alternatives considered

1. **All templates in `templates/`, including skill/agent skeletons.**
   - Rejected: a skill skeleton is a directory with nested optional files (`references/`, `scripts/`, `prompts/`, `tests/`). Moving that into `templates/` would force either a flattened lossy template or a non-obvious nested structure.
2. **All templates co-located by type.**
   - Rejected: would scatter document templates (ADR template under `adr/`, memory templates under `memories/`, etc.), making it harder to answer "what templates exist?" and increasing duplication risk.
3. **Hybrid — chosen.** Document templates central; package skeletons co-located.

## Consequences

### Positive
- Single place to find document templates.
- Skill and agent templates live next to the specs that describe them.
- Clear separation between "a document I fill in" and "a package I create."

### Negative
- Two conventions to remember. Documented in `templates/README.md` and each framework README.

## References

- `../templates/README.md`
- `../skills/README.md`
- `../agents/README.md`
