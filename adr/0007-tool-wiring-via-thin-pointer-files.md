---
id: ADR-0007
title: Tool wiring via thin pointer files
status: accepted
date: 2026-08-14
project: system
related: []
---

# ADR-0007: Tool wiring via thin pointer files

## Context

The system is meant to be vendor-neutral and usable across Claude Code, Kimi Code, OpenCode, Codex CLI, Cursor, Roo Code, Cline, and Gemini CLI. Different tools load global instructions from different files.

## Problem

How do we make `~/.agents/AGENTS.md` the single source of truth while supporting tools that expect their own global instruction files?

## Decision

- For tools with a known global instruction location, create a **thin pointer file** that instructs the tool to read and follow `~/.agents/AGENTS.md` and load referenced rules/workflows on demand.
- Tools with reliable global files: Claude Code (`~/.claude/CLAUDE.md`), Codex CLI (`~/.codex/AGENTS.md`), Gemini CLI (`~/.gemini/GEMINI.md`), OpenCode (`~/.config/opencode/AGENTS.md`), Cursor (`~/.cursor/rules/agents-os.mdc`).
- For project-scoped tools (Cline, Roo Code), provide ready-to-use snippets to paste into project-level instruction files (`/.clinerules`, `.roo/rules/agents-os.md`).
- Future agents are instructed generically to point at `~/.agents/AGENTS.md`.

## Alternatives considered

1. **Create one `AGENTS.md` and let each tool figure it out.**
   - Rejected: many tools won't load a file outside their expected path; the brain would be inert.
2. **Generate tool-specific full copies of the constitution.**
   - Rejected: duplicates content and creates drift risk.
3. **Symlink tool files to `~/.agents/AGENTS.md`.**
   - Rejected: some tools expect specific formats (Cursor `.mdc` with frontmatter) or directories that don't exist; symlinks also complicate per-tool formatting.
4. **Thin pointers for global tools + snippets for project-scoped tools — chosen.**

## Consequences

### Positive
- `~/.agents/AGENTS.md` remains the single source of truth.
- Tool wiring is documented and reproducible.
- Honest coverage: we don't pretend tools have global files they don't.

### Negative
- Pointer files live outside the repo and are not versioned here.
- Users must paste snippets for Cline/Roo per project.

## References

- `../system/tool-wiring.md`
- `../AGENTS.md`
