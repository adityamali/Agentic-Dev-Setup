# Skill Specification

This is the standard format for a skill in this repository. Every skill, whether lock-managed or locally authored, should follow this shape.

## File layout

```text
skills/<skill-name>/
├── SKILL.md              # required; this is the entry point
├── README.md             # optional; deeper overview for humans
├── references/           # optional; quick refs, examples, gotchas
│   └── *.md
├── scripts/              # optional; helper scripts (must be safe to inspect)
│   └── *.sh
├── prompts/              # optional; reusable prompts specific to this skill
│   └── *.md
└── tests/                # optional; validation checklists or test plans
    └── *.md
```

## `SKILL.md` structure

### 1. Frontmatter

```yaml
---
name: <skill-name>
description: One-line description of what the skill is for and when to use it.
version: 0.1.0
author: <name or org>
compatible_agents: [claude, codex, opencode, cursor, gemini]
dependencies: [<other-skill-name>, ...]
tags: [kebab-case, tags]
---
```

### 2. Purpose

What problem this skill solves and who should load it.

### 3. Prerequisites

What must be true before the skill can be used (tools installed, accounts, env vars, MCP servers).

### 4. Instructions

The core know-how. Use imperative statements. Prefer retrieval over pre-training when facts change. Break into phases if the skill is procedural.

### 5. Examples

Concrete examples of applying the skill. Use real-looking but safe values; never use real secrets.

### 6. Checklist

A self-check the agent can run before declaring the task done.

### 7. References

Links to authoritative docs, internal references, or related skills/memory notes.

### 8. Reusable prompts

Optional prompts stored in `prompts/` and referenced here.

## Rules for authoring

- Keep `SKILL.md` focused. If it exceeds ~400 lines, split deep reference into `references/`.
- Use the same frontmatter conventions as `../system/conventions.md`.
- Do not include secrets, real credentials, or user-specific paths.
- A skill must be loadable without side effects. Scripts in `scripts/` are optional and must be safe to inspect before running.
- Name the directory in kebab-case and match the `name` field exactly.

## Loading a skill

1. Read `SKILL.md`.
2. Check prerequisites.
3. Load dependencies (in declared order).
4. Follow instructions.
5. Run the checklist before finishing.
