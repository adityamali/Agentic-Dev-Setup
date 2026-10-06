# Workflows

Reusable engineering processes. Use a workflow as a checklist whenever the task matches. Each workflow ends with explicit hooks to update memory, ADRs, and architecture docs.

## How to use

1. Identify the closest workflow.
2. Read the **Trigger** section — if it doesn't match, pick another or fall back to `AGENTS.md` §6 (core operating rhythm).
3. Execute the phases in order. Don't skip validation, testing, or documentation hooks.
4. Cite relevant rules by ID instead of restating them.

## Index

| ID | Workflow | Trigger |
|----|----------|---------|
| WF-01 | [implement-feature.md](implement-feature.md) | Adding new behavior |
| WF-02 | [fix-bug.md](fix-bug.md) | Fixing a defect |
| WF-03 | [refactor-module.md](refactor-module.md) | Restructuring without behavior change |
| WF-04 | [create-api.md](create-api.md) | Designing/implementing an API |
| WF-05 | [database-migration.md](database-migration.md) | Changing schema or data |
| WF-06 | [performance-optimization.md](performance-optimization.md) | Improving speed/resource use |
| WF-07 | [security-review.md](security-review.md) | Reviewing for security issues |
| WF-08 | [architecture-change.md](architecture-change.md) | Changing structure or boundaries |
| WF-09 | [code-review.md](code-review.md) | Reviewing someone else's change |
| WF-10 | [nextjs-audit.md](nextjs-audit.md) | Auditing and fixing a Next.js project |
| WF-11 | [implement-multiple-features.md](implement-multiple-features.md) | Delivering a batch of related features (list, order, architecture) |

## Creating a new workflow

If a repeated task lacks a workflow, use [`_template.md`](_template.md), remove the example phases, and add the new workflow to this README. Consider recording the addition in `system/changelog.md`.
