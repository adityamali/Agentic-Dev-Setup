# Architecture

Framework for documenting project architecture. This is the canonical home for structured architecture docs. Everything here is global to the brain; per-project docs live under `<slug>/`.

## What a project architecture doc set covers

Each project should eventually have documentation covering:

1. **System overview** — what it does and who it serves.
2. **Components** — deployable/logical units and responsibilities.
3. **Dependencies** — internal and external, with direction.
4. **Service boundaries** — contracts and what is not shared.
5. **Data flow** — how data moves through the system.
6. **Deployment** — environments, build, ship, rollback.
7. **Technology stack** — languages, frameworks, datastores, infra.
8. **Shared modules** — what is shared and how it's governed.
9. **External integrations** — third parties, failure behavior, security.

For small projects, a single `architecture/<slug>/overview.md` may cover all of these. For larger systems, split into focused files (`overview.md`, `components.md`, `dataflow.md`, `deployment.md`, etc.) and link them.

## Layout

```text
architecture/
├── INDEX.md
└── <project-slug>/
    ├── overview.md
    ├── components.md
    ├── dataflow.md
    └── ...
```

## Boundary with memories

- `architecture/<slug>/` = the validated, decision-backed description of the system.
- `memories/projects/<slug>.md` = evolving observations, constraints, and running knowledge.

Link them both ways.

## When to update

Update architecture docs when you:

- Add/remove/rename a component or service.
- Change a significant data flow.
- Add or replace a dependency.
- Modify a service boundary.
- Change deployment, infrastructure, or external integrations.

## Template

Use [`../templates/architecture.md`](../templates/architecture.md).

## Registry

See [`INDEX.md`](INDEX.md) for the list of documented projects.
