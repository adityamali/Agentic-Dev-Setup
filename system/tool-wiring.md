# Tool Wiring

How each AI coding tool loads the global `../AGENTS.md`. This is the practical layer that makes the system vendor-*usable*, not just vendor-neutral.

## Global-instruction tools

These tools have a well-known global instruction file. We create a thin pointer that loads `~/.agents/AGENTS.md` and its referenced standards.

| Tool | Global file | Status |
|------|-------------|--------|
| Claude Code | `~/.claude/CLAUDE.md` | pointer to create |
| Codex CLI | `~/.codex/AGENTS.md` | pointer to create |
| Gemini CLI | `~/.gemini/GEMINI.md` | pointer to create |
| OpenCode | `~/.config/opencode/AGENTS.md` | pointer to create |
| Cursor | `~/.cursor/rules/agents-os.mdc` | pointer to create |

### Pointer file content

Use this content for `CLAUDE.md`, `AGENTS.md` (Codex), and `GEMINI.md`:

```markdown
# Global agent instructions

Read and follow `~/.agents/AGENTS.md` for all engineering standards.
Load referenced rules from `~/.agents/rules/` and use the relevant workflow
from `~/.agents/workflows/` when applicable. Treat `~/.agents/` as the
long-term engineering brain.
```

For OpenCode (`~/.config/opencode/AGENTS.md`):

```markdown
# Global agent instructions

Read and follow `~/.agents/AGENTS.md`. Use `~/.agents/rules/` for citable
standards and `~/.agents/workflows/` for repeatable processes.
```

For Cursor (`~/.cursor/rules/agents-os.mdc`):

```markdown
---
description: Global agent OS instructions
glob: **/*
---

Read and follow `~/.agents/AGENTS.md` for engineering standards. Use
`~/.agents/rules/` for citable rules and `~/.agents/workflows/` for checklists.
```

## Project-scoped tools

These tools load instructions per workspace/project rather than globally. Use the snippets below in each project where needed.

### Cline

Add to the project's `.clinerules`:

```markdown
# Global agent instructions

Read and follow `~/.agents/AGENTS.md` for engineering standards.
Use `~/.agents/rules/` for citable rules and `~/.agents/workflows/`
for repeatable processes. Maintain `~/.agents/memories/` for long-term
knowledge and create ADRs in `~/.agents/adr/` for consequential decisions.
```

Cline also supports custom instructions in VS Code settings; paste the same text there if preferred.

### Roo Code

Create `.roo/rules/agents-os.md` in the workspace:

```markdown
# Global agent instructions

Read and follow `~/.agents/AGENTS.md`. Use `~/.agents/rules/` and
`~/.agents/workflows/` as needed. Update `~/.agents/memories/`,
`~/.agents/adr/`, and `~/.agents/architecture/` when work produces
knowledge, decisions, or structural changes.
```

## Kimi Code and future agents

For any agent that supports a global instructions file, create a file pointing to `~/.agents/AGENTS.md` with the same pointer text. If the tool reads project-level instructions, use the Cline/Roo snippet. The goal is one source of truth: `~/.agents/AGENTS.md`.

## Skill mirrors

Some tools keep their own skill directories (e.g., `~/.claude/skills/`). This repo's `~/.agents/skills/` is the source of truth. If a skill is installed or authored here, mirror it to the tool's directory only via your chosen sync mechanism. Do not edit skills in both places.

## Updating wiring

When `../AGENTS.md` or the folder structure changes significantly, update the pointer files. This file is the registry of what needs changing.
