---
id: WF-09
title: Code review
---

# WF-09: Code review

## Trigger

Reviewing a code change produced by another engineer or agent.

## Phase 1 — Understand the change

- [ ] Read the description/PR summary and link to the issue/spec/ADR.
- [ ] Read the diff in logical order, not just file order.
- [ ] Identify the goal and the non-goals of the change.

## Phase 2 — Check correctness

- [ ] Does the change do what it claims?
- [ ] Are edge cases and error paths handled?
- [ ] Are there race conditions, off-by-one issues, or state mutations?
- [ ] Does the change preserve existing behavior where it should?

## Phase 3 — Check quality

- [ ] Is the change minimal and focused (CQ-12)?
- [ ] Are names clear and responsibilities single (CQ-02, CQ-03)?
- [ ] Is there duplicated logic or speculative abstraction (CQ-04, CQ-06)?
- [ ] Is error handling deliberate (CQ-10)?
- [ ] Are there AI-generated slop comments or dead code (CQ-08, CQ-07)?

## Phase 4 — Check architecture & security

- [ ] Do dependencies point the right way (AR-01)?
- [ ] Are boundaries respected (AR-03)?
- [ ] Is input validated at the boundary (SC-02)?
- [ ] Are secrets, auth, and authorization handled correctly?

## Phase 5 — Check tests & docs

- [ ] Do tests cover the change, including error paths (TS-01, TS-02)?
- [ ] Is there a regression test for the bug if this is a fix (TS-03)?
- [ ] Are docs updated if behavior changed (DOC-01)?
- [ ] Is an ADR needed but missing?

## Phase 6 — Summarize

- [ ] Provide a clear review: approve, request changes, or comment with blockers vs. nits.
- [ ] Cite specific rules by ID where applicable.
- [ ] Suggest concrete changes, not vague complaints.

## Exit criteria

- [ ] The change is correct, focused, tested, and documented.
- [ ] Security and architecture concerns are addressed or escalated.
- [ ] Review feedback is actionable.
