---
id: WF-03
title: Refactor module
---

# WF-03: Refactor module

## Trigger

Changing the internal structure of code without changing its external behavior.

## Phase 1 — Understand current behavior

- [ ] Identify the module's public contract (functions, types, events).
- [ ] Read tests to learn the expected behavior.
- [ ] Note why the current structure is problematic (duplication, poor names, wrong boundaries).
- [ ] Confirm no behavior changes are intended.

## Phase 2 — Plan the target structure

- [ ] State the single goal of the refactor.
- [ ] Identify affected callers and downstream tests.
- [ ] Choose whether to do it incrementally or in one change. Prefer incremental for large refactorings.
- [ ] Get user confirmation for non-trivial refactorings.

## Phase 3 — Refactor

- [ ] Keep the public contract stable unless you have explicit permission to change it.
- [ ] Make one mechanical change at a time (rename, extract, move).
- [ ] Run tests after each meaningful step.
- [ ] Delete dead code as you find it (CQ-07).

## Phase 4 — Validate

- [ ] All existing tests pass without changing assertions (TS-01).
- [ ] Static analysis passes.
- [ ] Manual spot-check if the module has behavioral gaps in test coverage.

## Phase 5 — Documentation

- [ ] Update comments if invariants changed.
- [ ] Update architecture docs if boundaries or component descriptions changed.
- [ ] Create/update memory if a lesson about structure emerged.
- [ ] ADR only if the refactor changes a cross-team or cross-service contract.

## Exit criteria

- [ ] Behavior is unchanged (all tests pass, no public API breakage).
- [ ] Code is measurably simpler or better bounded.
- [ ] No unrelated changes are bundled.
