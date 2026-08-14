# Templates

The single source of truth for document shapes. Copy a template, fill it in, and delete every `<!-- guidance -->` comment before saving. Do not leave guidance comments in real documents.

Frontmatter fields, tag rules, and statuses are defined in `../system/conventions.md` — templates assume them, they don't redefine them.

## Index

### Decisions & design
| Template | Use for | Output location |
|----------|---------|-----------------|
| [adr.md](adr.md) | Architecture Decision Records | `../adr/NNNN-slug.md` |
| [engineering-proposal.md](engineering-proposal.md) | Proposing a significant change before building it | `../adr/` or project docs |
| [architecture.md](architecture.md) | A project's architecture document set | `../architecture/<project-slug>/` |
| [feature-spec.md](feature-spec.md) | Specifying a feature before implementation | project docs |

### Knowledge & memory
| Template | Use for | Output location |
|----------|---------|-----------------|
| [memory-permanent.md](memory-permanent.md) | Evergreen engineering knowledge | `../memories/permanent/` |
| [memory-project.md](memory-project.md) | A running note on one project | `../memories/projects/` |
| [memory-technology.md](memory-technology.md) | Knowledge about one technology/tool | `../memories/technologies/` |
| [memory-lesson.md](memory-lesson.md) | A distilled lesson learned | `../memories/lessons/` |
| [memory-debugging.md](memory-debugging.md) | A debugging case (symptom → cause → fix) | `../memories/debugging/` |
| [research-note.md](research-note.md) | Time-stamped research / exploration | `../memories/research/` |

### Records & communication
| Template | Use for | Output location |
|----------|---------|-----------------|
| [project-summary.md](project-summary.md) | A concise brief on a project's current state | project docs |
| [bug-report.md](bug-report.md) | Reporting a defect before fixing it | project docs / issues |

## Conventions

- **Package skeletons are not here.** The skill template lives at `../skills/_template/` and the agent template at `../agents/_template/` — they are structures, not documents.
- Templates are starting points, not contracts. Omit a section that has nothing to say (per DOC-07); never ship an empty heading.
- Guidance comments use `<!-- ... -->`. Strip them all on instantiation.
