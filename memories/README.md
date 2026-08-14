# Memories

The long-term knowledge base. Markdown only, Obsidian-style: `[[wikilinks]]`, tags, Maps of Content (MOCs), and a clear lifecycle. No source code lives here; knowledge does.

## Taxonomy

| Folder | Holds | Template |
|--------|-------|----------|
| `permanent/` | Evergreen engineering knowledge: principles, patterns, definitions | [`templates/memory-permanent.md`](../templates/memory-permanent.md) |
| `projects/` | One note per project: evolving knowledge, constraints, quirks | [`templates/memory-project.md`](../templates/memory-project.md) |
| `technologies/` | Per-technology notes: gotchas, patterns, versions | [`templates/memory-technology.md`](../templates/memory-technology.md) |
| `research/` | Time-stamped exploration before conclusions are stable | [`templates/research-note.md`](../templates/research-note.md) |
| `lessons/` | Distilled lessons learned, promoted from projects or debugging | [`templates/memory-lesson.md`](../templates/memory-lesson.md) |
| `debugging/` | Symptom → cause → fix history | [`templates/memory-debugging.md`](../templates/memory-debugging.md) |
| `archive/` | Obsolete or superseded notes | — |

**Where engineering knowledge goes:** the "engineering knowledge" category lives in `permanent/` — these are evergreen, validated pieces of engineering understanding.

## Linking convention

- Inside `memories/`, use `[[wikilinks]]`: `[[postgres-connection-pooling]]`.
- Filenames are unique across all of `memories/`; the wikilink target is the filename without `.md`.
- **Maintain backlinks.** When note A links to note B, add A to B's `## Related` section. Do this manually; plain markdown files don't auto-backlink.
- Outside `memories/`, link with standard relative paths per [`system/conventions.md`](../system/conventions.md).

## Tags

- kebab-case, flat namespace.
- Apply in frontmatter, not inline.
- 1–5 tags per note.
- Reuse existing tags from `INDEX.md` before inventing new ones.

## Lifecycle

### Create a note when

- You learned something non-obvious that the next engineer/agent should know.
- You solved a bug and the pattern is worth preserving.
- A project reveals a constraint, quirk, or stable pattern.
- You validated a technology assumption (gotcha, limitation, version behavior).
- A decision has become stable knowledge (promote from ADR or research note).

### Update a note when

- Its facts change.
- A new project applies the concept (add a backlink).
- You discover a better formulation.

### Archive a note when

- It is factually obsolete and no one references it.
- Its content has been superseded by another note or an ADR.
- A research note's findings were promoted to permanent/project/technology/lesson.

To archive: move the file to `archive/`, set `status: archived`, and update `updated`.

## What belongs somewhere else

| If it is… | Put it in… |
|-----------|------------|
| A reusable how-to for a technology/domain | [`../skills/`](../skills/) as a skill |
| A documented decision with trade-offs | [`../adr/`](../adr/) as an ADR |
| A project's formal structure, components, or deployment | [`../architecture/<slug>/`](../architecture/) |
| A project's operational runbook | The project repo or `architecture/<slug>/` |
| A transient thought or scratch note | A scratchpad outside this repo |

## Index

The living map of content is [`INDEX.md`](INDEX.md). Keep it updated as notes grow.
