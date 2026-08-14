---
id: ADR-0001
title: Router-style AGENTS.md and per-folder README indexes
status: accepted
date: 2026-08-14
related: [ADR-0004]
---

# ADR-0001: Router-style AGENTS.md and per-folder README indexes

## Context

This repository is meant to be loaded into every AI coding session as the source of truth for engineering standards, memory, and process. At the same time, it must remain human-readable, searchable, and maintainable for years. The primary instruction file (`AGENTS.md`) competes for context-window space with the actual project code.

## Problem

How should the top-level instruction file be structured so that every agent reads it every session without it becoming a bloated, unmaintainable monolith?

## Decision

We will use a **router-style** `AGENTS.md`:

- `AGENTS.md` states philosophy, operating rhythm, and protocols in ~300 lines.
- Depth lives in citable rule files under `rules/` (e.g., `CQ-03`, `AR-02`).
- Every subdirectory has a `README.md` that serves as its index and routing guide.
- Agents load `AGENTS.md` first, then pull in detailed rule/workflow files on demand.

## Alternatives considered

1. **Single comprehensive `AGENTS.md`** — everything inline.
   - Rejected: would exceed practical context-window limits and duplicate content with rule files.
2. **No top-level file; rely on tool-specific instructions pointing at subdirectories**.
   - Rejected: every tool needs a single entry point; subdirectories fragment the initial load.
3. **Concise router `AGENTS.md`** — chosen. Keeps the entry point stable and small while allowing deep, citable rules.

## Consequences

### Positive
- Low per-session token cost.
- Rules and processes can evolve without rewriting the constitution.
- Citable IDs make feedback precise ("per CQ-03").

### Negative
- Agents must follow links rather than having everything inline; a poorly implemented agent might skip the rules.
- We must keep `AGENTS.md` and `rules/` in sync.

## References

- `../AGENTS.md`
- `../rules/README.md`
- `../system/conventions.md`
