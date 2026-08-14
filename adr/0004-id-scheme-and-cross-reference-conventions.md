---
id: ADR-0004
title: ID scheme and cross-reference conventions
status: accepted
date: 2026-08-14
related: [ADR-0001, ADR-0003]
---

# ADR-0004: ID scheme and cross-reference conventions

## Context

The repository contains many kinds of documents: rules, workflows, ADRs, memory notes, prompts, templates. Agents and humans need a compact way to cite a document without restating it.

## Problem

How do we make every durable artifact citable, searchable, and linkable without forcing a single link style everywhere?

## Decision

- Assign stable IDs to durable artifacts:
  - ADRs: `ADR-NNNN`
  - Rules: `CQ-NN`, `AR-NN`, `TS-NN`, `SC-NN`, `DOC-NN`
  - Workflows: `WF-NN`
- Use **standard relative Markdown links** across the whole repo for structural navigation.
- Use **`[[wikilinks]]`** only inside `memories/` to stay Obsidian-compatible.
- Cross-reference liberally but never duplicate content; cite IDs instead of restating rules.
- Frontmatter schemas and statuses are standardized per document type.

## Alternatives considered

1. **Wikilinks everywhere.**
   - Rejected: wikilinks are not rendered by GitHub and fail in tools that don't support them; structural docs need to be readable anywhere.
2. **Markdown links everywhere (including memories).**
   - Rejected: breaks Obsidian graph view and the user's explicit Obsidian-style requirement.
3. **One universal ID scheme for everything (including prompts/templates).**
   - Rejected: overkill; prompts and templates are naturally identified by name.
4. **Two-link dialect + scoped IDs — chosen.**

## Consequences

### Positive
- GitHub-compatible navigation for the framework.
- Obsidian graph for the knowledge base.
- Citable rules make reviews and ADRs precise.

### Negative
- Authors must remember which link dialect to use in which folder.
- Manual backlink maintenance inside memories.

## References

- `../system/conventions.md`
- `../rules/README.md`
- `../workflows/README.md`
