---
id: WF-08
title: Architecture change
---

# WF-08: Architecture change

## Trigger

Changing components, service boundaries, data flows, deployment topology, or introducing/removing a major dependency.

## Phase 1 — Understand current state

- [ ] Read `architecture/<slug>/` docs and related ADRs.
- [ ] Read the code that implements the current boundaries.
- [ ] Identify the problem with the current architecture.

## Phase 2 — Explore options

- [ ] List at least two real alternatives.
- [ ] Evaluate each against the project's constraints: team size, load, ops cost, time.
- [ ] Note the reversibility of each option.

## Phase 3 — Decide

- [ ] Choose the option that best balances simplicity, correctness, and future options.
- [ ] Create an ADR before implementing (AR-10). This is non-optional for architecture changes.
- [ ] Get user confirmation for the ADR before implementation.

## Phase 4 — Implement incrementally

- [ ] Break the change into the smallest safe increments.
- [ ] Maintain backward compatibility at each increment where possible.
- [ ] Update tests, interfaces, and dependency direction as you go (AR-01).
- [ ] Stop and re-plan if the change reveals unknown coupling.

## Phase 5 — Validate

- [ ] Run integration tests across affected boundaries.
- [ ] Verify deployment/rollout behavior.
- [ ] Confirm monitoring and alerting still apply.

## Phase 6 — Document

- [ ] Update all affected `architecture/<slug>/` files.
- [ ] Link the implementation to the ADR.
- [ ] Update `architecture/INDEX.md` if the project listing changed.
- [ ] Create/update memory notes for non-obvious consequences.

## Exit criteria

- [ ] ADR exists and is accepted.
- [ ] Architecture docs reflect the new state.
- [ ] All affected boundaries are tested.
- [ ] Implementation is incremental and reviewable.
