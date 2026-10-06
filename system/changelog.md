# Changelog

Dated log of structural changes to this AI engineering operating system. Content additions (memory notes, ADRs, architecture docs) are not logged here unless they change the system's structure.

## Format

```text
YYYY-MM-DD — Change description (affected file)
```

## Log

<!-- Add new entries at the top. -->

2026-08-15 — Add Next.js audit capability: locally authored `nextjs` skill (`skills/nextjs/SKILL.md`) and WF-10 workflow (`workflows/nextjs-audit.md`) for diagnosing and fixing Next.js imports, types, config, routing, and build issues. Update workflows index and skill registry.

2026-08-14 — Initialize the AI Engineering Operating System: AGENTS.md, rules, workflows, memories framework, architecture framework, ADR methodology with 7 seed ADRs, prompts, templates, agent/skill frameworks, system conventions/registry/maintenance/tool-wiring/changelog, .gitignore, and tool pointer files. Remove unused sync/ directory.
