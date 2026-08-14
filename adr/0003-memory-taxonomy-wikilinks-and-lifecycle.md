---
id: ADR-0003
title: Memory taxonomy, wikilinks, and lifecycle
status: accepted
date: 2026-08-14
related: [ADR-0001, ADR-0004]
---

# ADR-0003: Memory taxonomy, wikilinks, and lifecycle

## Context

The user requested an Obsidian-style long-term memory framework: markdown only, bidirectional links, indexes, tags, permanent notes, project notes, technology notes, research notes, lessons learned, and debugging history. The framework must start empty and define when notes are created, updated, archived, or moved elsewhere.

## Problem

How do we structure a memory system that is compatible with Obsidian, easy for agents to navigate, and disciplined enough not to become a dump of unrelated notes?

## Decision

- Folder-based taxonomy under `memories/`: `permanent/`, `projects/`, `technologies/`, `research/`, `lessons/`, `debugging/`, `archive/`.
- `[[wikilinks]]` inside `memories/` only; standard relative Markdown links everywhere else.
- Backlinks are maintained manually via a `## Related` section.
- Tags are kebab-case, flat, applied in frontmatter, limited to 1–5 per note.
- Lifecycle: create when knowledge is discovered, update when facts change, archive when obsolete, merge when duplicated.
- "Engineering knowledge" maps to `permanent/`.

## Alternatives considered

1. **Flat notes + tags only, no folders.**
   - Rejected: folders provide routing guidance for agents deciding where to file; tags alone would create classification disputes.
2. **Folders + standard Markdown links only (no wikilinks).**
   - Rejected: wikilinks are the Obsidian standard and make the knowledge graph visible to Obsidian users.
3. **Auto-backlinks via tooling.**
   - Rejected: this system is plain Markdown and tool-agnostic. Manual backlinks keep it simple and inspectable.
4. **Hybrid folders + wikilinks + manual backlinks — chosen.**

## Consequences

### Positive
- Works in Obsidian and any Markdown editor.
- Clear boundaries for where a note belongs.
- Backlinks preserve discoverability without tooling.

### Negative
- Manual backlinks require discipline; agents must maintain them.
- Filenames must be unique across all of `memories/` for wikilinks to resolve unambiguously.

## References

- `../memories/README.md`
- `../memories/INDEX.md`
- `../system/conventions.md`
