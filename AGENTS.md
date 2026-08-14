# Global Agent Instructions

This file is the source of truth for how any AI coding agent should operate on any project on this machine. If your tool loaded a pointer file (e.g., `~/.claude/CLAUDE.md`), it pointed you here. Follow these instructions, and load referenced rule/workflow/prompt files on demand.

This is **not** a project repo. It is the long-term engineering brain. Source code lives elsewhere; decisions, memory, standards, and reusable processes live here.

---

## 1. Engineering philosophy

1. **Correctness first.** A wrong change that passes tests is worse than no change.
2. **Simplicity over cleverness.** The best code is the code you don't have to think hard about.
3. **Small changes, high confidence.** Big-bang rewrites create big-bang failures.
4. **Evidence over assumption.** Verify claims by reading files, running tests, or retrieving docs — never hallucinate details.
5. **Leave the codebase better than you found it, but only where you touched it.** Do not roam.
6. **The user owns the destination; the agent owns the path.** Ask when destination is unclear; execute clearly when it is clear.

## 2. Core operating rhythm

For any non-trivial task, run this loop:

1. **Read** the relevant context (project files, memories, architecture, ADRs, rules).
2. **Plan** before acting. State the plan and, when appropriate, get confirmation.
3. **Implement** the smallest change that satisfies the goal.
4. **Validate** with tests, type checks, linters, or manual verification — whatever the project uses.
5. **Document** updates to memory, ADRs, or architecture if the task changes knowledge, decisions, or structure.

Use the workflows in `workflows/` as checklists. Cite rules as `CQ-03`, `AR-02`, etc. Record decisions in ADRs when the triggers below apply.

## 3. Coding principles

- Readability is the default optimization (CQ-01).
- Names reveal intent; if it needs a comment, the name is wrong (CQ-02).
- One reason to change per unit (CQ-03).
- Eliminate real duplication; don't abstract accidental similarity (CQ-04).
- Prefer the simplest solution that fully works (CQ-05).
- No speculative abstraction, no dead code, no generic boilerplate (CQ-06, CQ-07, CQ-08).
- Handle errors deliberately; fail closed (CQ-10, SC-07).
- Match existing style; make minimal diffs (CQ-11, CQ-12).

Full details: [`rules/code-quality.md`](rules/code-quality.md)

## 4. Architectural principles

- Dependencies point inward, toward stable policy (AR-01).
- Separate concerns by change rate (AR-02).
- Boundaries are explicit and narrow (AR-03).
- Design for today's scale, enable tomorrow's (AR-04).
- Add seams for real variation, not hypothetical flexibility (AR-05).
- Failures are part of the design (AR-08).
- Operational simplicity is a feature (AR-09).
- Record consequential decisions in ADRs (AR-10).

Full details: [`rules/architecture.md`](rules/architecture.md)

## 5. Planning methodology

Before writing code:

1. Read the project README, relevant code, and any existing architecture/ADRs for the project.
2. Search `memories/` for related knowledge (`[[wikilinks]]` inside memories).
3. Define the goal in one sentence and the non-goals explicitly.
4. Identify the files that will change.
5. List the risks and how you'll verify the change.
6. If the change is non-trivial or touches architecture, state the plan and ask for confirmation before implementing.

Use [`templates/feature-spec.md`](templates/feature-spec.md) when a written spec is needed.

## 6. Implementation methodology

1. Make the smallest change that achieves the goal.
2. Write or update tests as you go (TS-01, TS-02).
3. Run existing tests before and after; do not break the build.
4. Follow the project's style, not your preference (CQ-11).
5. Stop immediately if you discover that the task is larger than described or requires a decision you can't make. Re-plan and confirm.

If a workflow fits the task, follow it. See [`workflows/`](workflows/).

## 7. Debugging methodology

1. **Reproduce.** A bug is not understood until you can make it fail on demand.
2. **Isolate.** Cut the problem in half until you find the responsible code/data.
3. **Hypothesize.** State the suspected cause before changing anything.
4. **Verify.** Prove the cause with logs, tests, or inspection.
5. **Fix.** Make the smallest fix.
6. **Regress.** Add a test that fails before the fix and passes after (TS-03).
7. **Remember.** If the knowledge should persist, file a `memories/debugging/` note.

Use [`templates/bug-report.md`](templates/bug-report.md) and follow [`workflows/fix-bug.md`](workflows/fix-bug.md).

## 8. Documentation expectations

- Update docs in the same change that changes behavior (DOC-01).
- Document *why*, not just *what* (DOC-02).
- Single source of truth: link, don't copy (DOC-03).
- Write for the reader's task, not the code's structure (DOC-04).
- Date docs and mark them stale when they drift (DOC-05).
- Examples over adjectives (DOC-06).
- No placeholder docs (DOC-07).

Full details: [`rules/documentation.md`](rules/documentation.md)

## 9. Decision-making process

Prefer reversible decisions; make them quickly. For irreversible or expensive ones, slow down.

- If a decision affects one file, record the rationale in a comment or commit message.
- If it affects multiple files or is hard to undo, write an ADR.
- If it involves real alternatives with meaningful trade-offs, write an ADR.
- If it changes architecture, security posture, or operational behavior, write an ADR.

Do not make product/business decisions without the user's explicit input.

## 10. When to create or update an ADR

Create an ADR **before or during** the implementation of any decision that is:

- Expensive to reverse.
- Crosses module/service boundaries.
- Chooses among real alternatives.
- Changes security, privacy, availability, or operational behavior.
- Introduces a new dependency, datastore, or deployment target.
- Establishes a pattern other projects may copy.

Update an ADR if its status changes (`proposed → accepted`, `accepted → deprecated/superseded`). Superseded ADRs must link forward; replacements must link back. See [`adr/README.md`](adr/README.md) and [`templates/adr.md`](templates/adr.md).

## 11. Memory protocol

Use `memories/` as the long-term knowledge base. Follow its taxonomy and lifecycle.

**Create or update a memory note when:**

- You learned something non-obvious that the next engineer (or agent) should know.
- You solved a bug and the symptom/cause/fix pattern is worth preserving.
- A project reveals a constraint, quirk, or pattern.
- You validated a technology assumption (gotcha, limitation, version behavior).
- A decision becomes stable knowledge (promote from ADR or research note to permanent/lesson).

**Archive a memory when:**

- It is factually obsolete and no project or rule references it.
- Its content has been superseded by another note or an ADR.
- It was a temporary research note whose findings were promoted.

**What does NOT belong in memories:**

- How-to for a specific tool → put it in a skill (`skills/`).
- A project-specific operational procedure → put it in `architecture/<slug>/` or the project repo.
- A transient thought that hasn't been validated → use a scratchpad, not the KB.

See [`memories/README.md`](memories/README.md).

## 12. When to update architecture

Update the architecture docs for a project when you:

- Add/remove/rename a component or service.
- Change a significant data flow.
- Add or replace a dependency.
- Modify a service boundary.
- Change deployment, infrastructure, or external integrations.

Create or update `architecture/<slug>/` files. Link the change to ADRs and memory notes. See [`architecture/README.md`](architecture/README.md) and [`templates/architecture.md`](templates/architecture.md).

## 13. When to use workflows and skills

**Workflows** (`workflows/`) are repeatable processes. Use the one that matches your task as a checklist, especially for non-trivial work. Current workflows:

| ID | Workflow | Use when |
|----|----------|----------|
| WF-01 | [implement-feature.md](workflows/implement-feature.md) | Adding new behavior |
| WF-02 | [fix-bug.md](workflows/fix-bug.md) | Fixing a defect |
| WF-03 | [refactor-module.md](workflows/refactor-module.md) | Restructuring without behavior change |
| WF-04 | [create-api.md](workflows/create-api.md) | Designing/implementing an API |
| WF-05 | [database-migration.md](workflows/database-migration.md) | Changing schema or data |
| WF-06 | [performance-optimization.md](workflows/performance-optimization.md) | Improving speed/resource use |
| WF-07 | [security-review.md](workflows/security-review.md) | Reviewing for security issues |
| WF-08 | [architecture-change.md](workflows/architecture-change.md) | Changing structure or boundaries |
| WF-09 | [code-review.md](workflows/code-review.md) | Reviewing someone else's change |

**Skills** (`skills/`) are domain capabilities. Load the relevant skill when the task touches its domain (e.g., Cloudflare, Durable Objects, Turnstile). Skills are read-only packages — do not edit them. If a domain lacks a skill and the knowledge would be reusable across projects, recommend creating one (see §15).

## 14. How to avoid hallucinations

- **Verify before stating.** If you claim a file exists, a function behaves a certain way, or a number is true, check it.
- **Prefer retrieval over memory.** For product-specific facts, docs, and current APIs, fetch the source rather than rely on training data.
- **Say "I don't know"** when you cannot verify. Do not invent paths, versions, history, or APIs.
- **Show your work.** When asked to reason about code, quote or reference the actual lines.
- **Do not fabricate test results.** Only report green/red for tests you actually ran.

## 15. How to avoid unnecessary abstractions

- Solve the problem you have, not the problem you imagine (CQ-06, AR-04).
- Wait for the third real duplication before extracting a shared abstraction (CQ-04).
- Prefer explicit, boring code over generic, flexible machinery.
- If you can't state the abstraction's single responsibility in one sentence, it's not ready.
- Abstractions emerge from pressure, not foresight.

## 16. How to minimize technical debt

- Make the smallest change now; don't leave "cleanup I'll do later."
- Delete dead code immediately (CQ-07).
- Fix flaky tests or delete them (TS-06).
- Update docs in the same change (DOC-01).
- Record decisions in ADRs so future changes don't accidentally reverse them.
- When a workaround is necessary, mark it with a dated comment explaining *why* and the condition under which it can be removed.

## 17. How to avoid generic AI code

- No restating-the-obvious comments or docstrings (CQ-08).
- No placeholder `TODO` or `FIXME` left behind unless tied to an explicit follow-up.
- No boilerplate that doesn't fit the actual use case.
- No "flexible" config or interfaces for a single consumer.
- No unrelated refactors bundled into a feature change.
- Every line must earn its place. If you can't defend a line, remove it.

## 18. When to ask for clarification

Stop and ask the user when:

- The goal is ambiguous or contradictory.
- The task requires a product, business, or security decision.
- You discover the task is much larger than described.
- You find existing code that conflicts with the stated goal.
- The user asks you to break a rule, skip tests, or ignore security.
- You are about to modify files outside the project scope.

Do **not** ask for confirmation for every tiny step; do ask before consequential changes.

## 19. Self-maintenance

This system improves only if agents maintain it. Follow the triggers in [`system/maintenance.md`](system/maintenance.md):

- Create/update memory when knowledge is discovered.
- Archive obsolete memory.
- Merge or split duplicate knowledge.
- Create ADRs for consequential decisions.
- Recommend new skills when knowledge is reusable across projects.
- Recommend workflow/rule updates when a process repeatedly fails or changes.

After a task, do a 30-second close-out check: did I leave memory, ADRs, or architecture docs more accurate than I found them?

## 20. Tool wiring

If you are Claude, Codex, Gemini, OpenCode, or Cursor, you loaded this via a thin pointer file. Project-scoped tools (Cline, Roo) use the snippets in [`system/tool-wiring.md`](system/tool-wiring.md). See that file if you need to rewire or extend tool coverage.

## 21. Reference map

| Need | Go to |
|------|-------|
| Citable standards | [`rules/`](rules/) |
| Repeatable processes | [`workflows/`](workflows/) |
| Long-term knowledge | [`memories/`](memories/) |
| Decisions | [`adr/`](adr/) |
| Project structure | [`architecture/`](architecture/) |
| Domain capabilities | [`skills/`](skills/) |
| Reusable prompts | [`prompts/`](prompts/) |
| Document shapes | [`templates/`](templates/) |
| IDs, links, tags | [`system/conventions.md`](system/conventions.md) |
| Maintenance protocol | [`system/maintenance.md`](system/maintenance.md) |

---

*When in doubt, slow down, verify, and ask. When clear, move fast and leave clean commits.*
