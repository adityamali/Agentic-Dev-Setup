---
id: WF-11
title: Implement multiple features
---

# WF-11: Implement multiple features

## Trigger

Delivering a batch of related features as one unit — an epic, a milestone, or a release slice — where the *list*, the *order*, and the *architecture* that holds them are decisions that matter beyond any single feature. Use WF-01 for a single feature; use this workflow when sequencing and cross-feature structure are part of the work.

## Preparation

- [ ] Read the project README and existing `architecture/<slug>/` docs.
- [ ] Search `memories/` for related project notes, technologies, and lessons.
- [ ] List the rules that apply (cite by ID, don't restate): `AR-01`, `AR-03`, `AR-04`, `AR-10`, `CQ-05`, `CQ-12`, `TS-01`, `DOC-01`, `DOC-03`.
- [ ] Decide whether the work is small enough to plan-and-go or needs a plan-and-confirm step (AGENTS.md §5, §9). For a multi-feature batch, default to plan-and-confirm.

## Phase 1 — Plan the feature list and the architecture

This phase produces the contract for the whole batch. Do not implement before it is confirmed.

- [ ] State the batch goal in one sentence and the non-goals explicitly (AGENTS.md §5.3).
- [ ] Enumerate the full feature list. For every feature that is non-trivial, write a spec from [`templates/feature-spec.md`](../templates/feature-spec.md). One feature = one spec = one acceptance list.
- [ ] Define the architecture that will hold the batch: components touched, service boundaries, and data flow (AR-01, AR-03, AR-04). If the batch changes structure or boundaries, run [`architecture-change.md`](architecture-change.md) (WF-08) *first* and get its ADR accepted before any feature work.
- [ ] Sequence the features by dependency: foundation first, dependents after. Record the order and the dependency edges. If a feature's spec can't be satisfied without another landing first, that edge is binding.
- [ ] Identify shared work or abstractions the batch implies — but only extract them when the duplication is real (CQ-04). Do not pre-build speculative shared modules (CQ-06, AR-06).
- [ ] List risks, cross-feature coupling, and the validation strategy for the batch as a whole.
- [ ] Get user confirmation on the feature list, the architecture, and the order before implementing. Treat the confirmed plan as the single source of truth (DOC-03): keep it in one place (e.g., the project repo or `architecture/<slug>/`) and link to it.

## Phase 2 — Implement features recursively (WF-01 loop)

Run WF-01 to completion for each feature, in the confirmed order. This phase is a loop with a termination condition, not a merge.

- [ ] Track an explicit checklist of the feature list. One feature at a time; mark each `[ ]` → `[x]` only when its own exit criteria are met.
- [ ] For the current feature, execute [`implement-feature.md`](implement-feature.md) (WF-01) end to end — planning through documentation, including its Phase 5 doc hooks.
- [ ] Implement the smallest slice that satisfies that feature's acceptance criteria (CQ-05) with a minimal diff (CQ-12). Match existing style (CQ-11).
- [ ] Validate *that* feature: run tests/type checks/linters, and prove tests run (TS-01, TS-10). Do not move to the next feature with a failing build.
- [ ] Stop and re-plan if a feature reveals that the plan, the architecture, or the order is wrong. Re-enter Phase 1 for the affected parts; do not silently drift from the confirmed plan.
- [ ] After each feature, update running state: the master checklist, any feature spec status, and per-feature memory/ADR hooks that WF-01 already requires.
- [ ] Loop to the next feature until every feature in the list is `[x]`.

## Phase 3 — Validate the batch

Phase 2 validates each feature in isolation; this phase validates the batch as a whole.

- [ ] Run the full test/type-check/lint suite, not just the slices touched per feature (TS-02).
- [ ] Check integration *between* the features — boundaries the plan assumed (AR-03) and dependencies the ordering relied on.
- [ ] Confirm no regressions and no unrelated files modified (CQ-12).
- [ ] Verify acceptance criteria across the batch, manually where automation is insufficient.

## Phase 4 — Update memory, architecture, and ADRs

Per-feature documentation is handled by WF-01 in Phase 2. This phase consolidates batch-level knowledge and decisions. Cite IDs; link, don't copy (DOC-03).

- [ ] **Architecture:** update `architecture/<slug>/` to reflect the structure that emerged across the batch — components, boundaries, data flow (AGENTS.md §12). Mark docs `stale` rather than leaving them wrong (DOC-05). Update [`architecture/INDEX.md`](../architecture/INDEX.md) if the project listing changed.
- [ ] **ADRs:** create or update ADRs for any consequential decision made during the batch — a cross-feature structural choice, a dependency introduction, a sequencing decision that was expensive to reverse — per the triggers in [`adr/README.md`](../adr/README.md) and AGENTS.md §10. If the architecture was pre-changed via WF-08, link these ADRs to that one.
- [ ] **Memory:** create or update memory notes for non-obvious knowledge discovered across the batch — project constraints, technology gotchas, and lessons worth promoting to `memories/lessons/`. Follow [`memories/README.md`](../memories/README.md) and AGENTS.md §11; maintain backlinks for any wikilinks you add inside `memories/`.
- [ ] Confirm every feature spec's status reflects reality (draft → done) and the master checklist is fully `[x]`.

## Exit criteria

- [ ] Every feature in the confirmed list is implemented and individually validated (all `[x]`).
- [ ] The batch passes validation as a whole, with no regressions.
- [ ] `architecture/<slug>/`, ADRs, and memory notes are updated to reflect the new state.
- [ ] The confirmed plan/specs, the architecture, and the actual implementation agree — no drift.
- [ ] The user is informed of what shipped, the order it shipped in, and why.