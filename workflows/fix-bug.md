---
id: WF-02
title: Fix bug
---

# WF-02: Fix bug

## Trigger

A defect has been reported: observed behavior differs from expected behavior.

## Phase 1 — Reproduce & triage

- [ ] Read the bug report or create one from [`templates/bug-report.md`](../templates/bug-report.md).
- [ ] Reproduce the failure. A bug that can't be reproduced is not ready to fix.
- [ ] Note environment, version, and exact error state.
- [ ] Search `memories/debugging/` for similar symptoms.

## Phase 2 — Isolate & diagnose

- [ ] Form a hypothesis about the cause.
- [ ] Cut the problem in half: logs, breakpoints, test cases, bisection.
- [ ] Prove the cause with evidence, not intuition.
- [ ] If the root cause spans systems, loop in the relevant architecture/ADR docs.

## Phase 3 — Fix

- [ ] Make the smallest fix that resolves the root cause, not the symptom.
- [ ] Do not bundle unrelated cleanup (CQ-12).
- [ ] Add or update error handling if the failure mode could recur (AR-08).

## Phase 4 — Regression test

- [ ] Write a test that fails before the fix and passes after (TS-03).
- [ ] Run the full relevant test suite to confirm no collateral damage.
- [ ] Run the original reproduction step and confirm it is fixed.

## Phase 5 — Documentation

- [ ] Fill in the bug report resolution section: root cause, fix, regression test.
- [ ] If the fix reveals a non-obvious pattern or gotcha, create a `memories/debugging/` note.
- [ ] If the fix changes architecture or a security boundary, create/update an ADR.

## Exit criteria

- [ ] Root cause is identified and documented.
- [ ] Fix is minimal and verified.
- [ ] Regression test exists and passes.
- [ ] Knowledge is preserved if it should persist.
