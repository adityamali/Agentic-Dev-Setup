# Conventions

The single source of truth for identifiers, links, tags, frontmatter, naming, and statuses across this repository. Every other document cites this file rather than restating it. When a convention changes, change it here and update the `system/changelog.md`.

---

## 1. Identifiers

Every durable artifact gets a stable, citable ID. Cite IDs instead of restating content.

| Scope | Prefix | Format | Example |
|-------|--------|--------|---------|
| Architecture Decision Record | `ADR` | `ADR-NNNN` (4-digit, zero-padded) | `ADR-0003` |
| Code-quality rule | `CQ` | `CQ-NN` | `CQ-03` |
| Architecture rule | `AR` | `AR-NN` | `AR-02` |
| Testing rule | `TS` | `TS-NN` | `TS-01` |
| Security rule | `SC` | `SC-NN` | `SC-04` |
| Documentation rule | `DOC` | `DOC-NN` | `DOC-01` |
| Workflow | `WF` | `WF-NN` | `WF-07` |

Rules for IDs:

- IDs are **never reused**. A retired rule or ADR keeps its ID; mark it `deprecated` or `superseded` (see §5).
- IDs are assigned sequentially within their scope. Gaps are fine; do not compact.
- Memory notes do **not** require IDs (their type is implicit in their folder). Use a descriptive filename instead.
- Prompts, templates, and skills are referenced by name, not ID.

## 2. Links — two dialects, strictly separated

This repository uses **two** link styles. Do not mix them within a context.

| Context | Dialect | Syntax | Why |
|---------|---------|--------|-----|
| Everywhere **except** `memories/` | Standard relative Markdown | `[ADR-0001](../adr/0001-router-style-agents-md-and-per-folder-readme-indexes.md)` | Renders on GitHub, survives plain `grep`, tool-agnostic |
| Inside `memories/` only | Wikilinks | `[[project-atlas]]` | Obsidian-compatible knowledge graph |

Wikilink rules (memories only):

- Use the note's filename (without extension) as the link target: `[[postgres-connection-pooling]]`.
- Wikilinks resolve within `memories/` regardless of subfolder — keep filenames unique across all of `memories/`.
- **Backlinks are manual.** When you add `[[b]]` to note `a`, also add `[[a]]` to note `b`'s `## Related` section. Obsidian does this automatically; plain files need the convention. Agents must maintain both directions.
- Never use a wikilink outside `memories/`; never use a relative Markdown link between two memory notes.

## 3. Tags

- kebab-case only: `#connection-pooling`, not `#ConnectionPooling`.
- Flat namespace. No hierarchical `#a/b/c` tags.
- Apply tags in frontmatter (see §4), not inline in prose.
- A note should carry 1–5 tags. More is noise.
- Reuse existing tags before inventing new ones. The canonical tag list lives in `memories/INDEX.md`; add new tags there when you introduce them.

Suggested starter taxonomy (extend as needed): `debugging`, `lesson`, `decision`, `performance`, `security`, `architecture`, `tooling`, `pattern`, `antipattern`, plus one tag per technology and per project as they appear.

## 4. Frontmatter

All structured documents use YAML frontmatter between `---` fences. Only the fields listed for a type are allowed — do not invent fields. Field order below is the canonical order.

**Memory note** (`memories/`)
```yaml
---
type: permanent | project | technology | research | lesson | debugging
title: Human-readable title
tags: [kebab-case, tags]
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active | archived
project: <slug>            # only for type: project / debugging tied to one project
---
```

**ADR** (`adr/`)
```yaml
---
id: ADR-NNNN
title: Short imperative title
status: proposed | accepted | deprecated | superseded
date: YYYY-MM-DD
project: <slug>            # omit for system-level ADRs
superseded_by: ADR-NNNN    # only when status: superseded
related: [ADR-NNNN, ...]   # optional
---
```

**Architecture doc** (`architecture/<slug>/`)
```yaml
---
project: <slug>
title: Document title
updated: YYYY-MM-DD
status: draft | current | stale
---
```

**Workflow / rule file / prompt** — these use in-file headers, not frontmatter. See each folder's README.

Dates are always ISO-8601 (`YYYY-MM-DD`). Update the `updated` field whenever you materially change a note.

## 5. Status lifecycles

**ADR:** `proposed → accepted → (deprecated | superseded)`
- `superseded` requires `superseded_by` pointing at the replacement ADR, and the replacement must list the old one in `related`.
- `deprecated` = no longer relevant, not replaced.

**Memory:** `active → archived` (move the file to `memories/archive/`, set `status: archived`).

**Architecture doc:** `draft → current → stale` (`stale` means "known out of date; needs refresh" — do not delete).

## 6. File & folder naming

- Filenames: kebab-case, `.md` only. `connection-pooling.md`, not `ConnectionPooling.md`.
- ADR filenames: `NNNN-kebab-slug.md` matching the ID (`0003-memory-taxonomy.md`).
- Memory note filenames: kebab-case, unique across all of `memories/` (wikilinks depend on this).
- Every directory contains a `README.md` that states its purpose, what belongs in it, and what does not. This is the routing layer for both humans and agents.
- Underscore-prefixed names (`_template`, `_template.md`) denote scaffolding, never content. Installers and indexers must skip them.

## 7. Cross-referencing

- Reference by ID where an ID exists (`per CQ-03`, `see ADR-0002`, `WF-04`).
- Reference files by repository-relative path from the repo root in prose: `rules/security.md`.
- Cross-reference liberally but never duplicate content: summarize-and-link, don't copy.
- If two documents seem to need the same paragraph, the paragraph lives in exactly one place and the other links to it.

---

*Governed by `system/maintenance.md`. Changes here are structural — record them in `system/changelog.md`.*
