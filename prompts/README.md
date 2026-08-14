# Prompts

Reusable prompt templates for common engineering tasks. Each prompt is generic: replace `{{LIKE_THIS}}` with project-specific values before using.

Use a prompt when you want consistent output shape or when handing a task to a different agent/tool. Do not treat these as magical incantations — they are structured starting points.

## Index

| Prompt | Use for |
|--------|---------|
| [architecture-review.md](architecture-review.md) | Review a system's architecture against standards |
| [debugging.md](debugging.md) | Structure a debugging session |
| [performance-analysis.md](performance-analysis.md) | Analyze a performance problem |
| [security-audit.md](security-audit.md) | Audit code/system for security issues |
| [feature-planning.md](feature-planning.md) | Plan a new feature |
| [migration-planning.md](migration-planning.md) | Plan a migration |
| [design-review.md](design-review.md) | Review a design or proposal |
| [refactoring.md](refactoring.md) | Plan or review a refactoring |

## Conventions

- Variables are in `{{DOUBLE_BRACES}}`.
- Sections in `<!-- comments -->` are instructions for the invoker, not part of the prompt text.
- Remove guidance comments before sending the final prompt.
