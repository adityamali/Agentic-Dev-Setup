---
id: WF-01
title: Implement feature
---

# WF-01: Implement feature

## Trigger

Adding new behavior, capability, or user-visible functionality.

## Phase 1 — Planning

- [ ] Read the project README and any existing `architecture/<slug>/` docs.
- [ ] Search `memories/` for related project notes, technologies, or lessons.
- [ ] State the goal and non-goals in one sentence each.
- [ ] Identify affected files and service boundaries.
- [ ] Decide whether to write a feature spec from [`templates/feature-spec.md`](../templates/feature-spec.md). Write one for non-trivial changes.
- [ ] List risks and validation strategy.
- [ ] Get user confirmation for non-trivial plans before implementing.

## Phase 2 — Implementation

- [ ] Create a branch / draft change set.
- [ ] Implement the smallest slice that satisfies the acceptance criteria (CQ-05).
- [ ] Match existing style; do not refactor unrelated code (CQ-12).
- [ ] Add error handling deliberately (CQ-10).
- [ ] Stop and re-plan if the scope grows or a consequential decision appears.

## Phase 3 — Validation

- [ ] Run tests / type checks / linters.
- [ ] Run the project locally if needed.
- [ ] Verify acceptance criteria manually when automated coverage is insufficient.

## Phase 4 — Testing

- [ ] Write tests that fail before the feature and pass after (TS-01, TS-02).
- [ ] Cover happy path, error path, and boundary conditions.
- [ ] Ensure no existing tests break.

## Phase 5 — Documentation

- [ ] Update user-facing docs if behavior changed (DOC-01).
- [ ] Update `architecture/<slug>/` if components/data flow changed.
- [ ] Create/update ADR if the feature required a consequential structural or dependency decision.
- [ ] Create/update memory notes for non-obvious project knowledge or gotchas.

## Exit criteria

- [ ] Feature works as specified.
- [ ] All validation passes.
- [ ] Docs/memory/ADR hooks satisfied.
- [ ] Change set is minimal and reviewable.
