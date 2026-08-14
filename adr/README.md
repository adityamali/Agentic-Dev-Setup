# Architecture Decision Records (ADRs)

A permanent, chronological log of significant engineering decisions. Each ADR records the context, problem, decision, alternatives, consequences, and status.

## Naming

`NNNN-kebab-slug.md`, matching the ADR ID. Examples:

- `0001-router-style-agents-md.md`
- `0012-use-postgres-for-job-queue.md`

## Required fields

See [`../templates/adr.md`](../templates/adr.md). Every ADR must contain:

- title, context, problem, decision
- alternatives considered (real ones, not strawmen)
- consequences (positive and negative)
- status and date
- `superseded_by` when applicable
- `related` ADRs

## Status lifecycle

```text
proposed → accepted → deprecated
              ↓
         superseded (by ADR-NNNN)
```

- `proposed` — under discussion, not yet binding.
- `accepted` — the decision is in force.
- `deprecated` — no longer relevant, not replaced.
- `superseded` — replaced by a newer ADR; must link forward, and the newer ADR must link back.

## When you MUST create an ADR

Create an ADR before or during implementation if the decision is:

- Expensive to reverse.
- Crosses module/service boundaries.
- Chooses among real alternatives.
- Changes security, privacy, availability, or operational behavior.
- Introduces a new dependency, datastore, or deployment target.
- Establishes a pattern other projects may copy.

## Project tagging

Because this brain stores all artifacts globally, every project-level ADR carries a `project` frontmatter field. System-level ADRs omit it. The `INDEX.md` sorts and filters by this field.

## Related documents

- Template: [`../templates/adr.md`](../templates/adr.md)
- Conventions: [`../system/conventions.md`](../system/conventions.md)
- Decision triggers in AGENTS.md: `../AGENTS.md` §10
- Workflow for architecture changes: [`../workflows/architecture-change.md`](../workflows/architecture-change.md)
