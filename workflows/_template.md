---
id: WF-00
title: Workflow template
---

# WF-00: Workflow title

## Trigger

<!-- When to use this workflow. Be specific enough that an agent can decide. -->

## Preparation

- [ ] Read relevant project context: README, architecture, ADRs, memories.
- [ ] Identify the rules that apply.
- [ ] Determine if this is trivial or needs a plan-and-confirm step.

## Phase 1 — Planning

- [ ] Define the goal in one sentence.
- [ ] Define non-goals.
- [ ] Identify affected files/components.
- [ ] List risks and how to mitigate them.
- [ ] Choose how to validate the result.

## Phase 2 — Implementation

- [ ] Make the smallest change that satisfies the goal.
- [ ] Follow the project's existing style.
- [ ] Stop and re-plan if scope grows.

## Phase 3 — Validation

- [ ] Run the relevant tests/type checks/linters.
- [ ] Verify the change manually if automated coverage is incomplete.
- [ ] Confirm no unrelated files were modified.

## Phase 4 — Testing

- [ ] Add or update tests for the new behavior.
- [ ] Ensure the test fails before the fix/feature and passes after.
- [ ] Check edge cases and error paths.

## Phase 5 — Documentation

- [ ] Update project docs if behavior changed.
- [ ] Update the relevant architecture doc if structure changed.
- [ ] Update or create an ADR if a consequential decision was made.
- [ ] Update or create a memory note if knowledge should persist.

## Exit criteria

<!-- Concrete state that means the workflow is done. -->

- [ ] Goal achieved.
- [ ] Validation passes.
- [ ] Docs/memory/ADR hooks satisfied.
- [ ] User is informed of what changed and why.
