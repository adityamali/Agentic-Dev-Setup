# System

The operating-system internals for this AI engineering brain. This folder holds the conventions, registries, maintenance protocol, tool-wiring instructions, and changelog — not the content itself.

## Files

| File | Purpose |
|------|---------|
| [conventions.md](conventions.md) | IDs, links, tags, frontmatter, naming, statuses |
| [registry.md](registry.md) | Installed skills (lock-managed + local) |
| [maintenance.md](maintenance.md) | When to create/archive/merge memory, ADRs, architecture, skills, workflows, rules |
| [tool-wiring.md](tool-wiring.md) | How each AI tool loads `../AGENTS.md` (global files + snippets) |
| [changelog.md](changelog.md) | Dated log of structural changes to this OS |

## Who edits what

- **Agents** edit content: memories, ADRs, architecture docs, prompts, workflows, rules.
- **Humans or agents** edit `system/` when the operating system itself needs to change.
- Any change to `system/` conventions is structural — record it in `changelog.md`.
