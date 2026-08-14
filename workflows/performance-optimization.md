---
id: WF-06
title: Performance optimization
---

# WF-06: Performance optimization

## Trigger

Improving latency, throughput, resource use, or scalability of an existing system.

## Phase 1 — Measure

- [ ] Identify the metric to improve and the target value.
- [ ] Profile or instrument to find the actual bottleneck.
- [ ] Do not optimize based on hunches. Quantify before changing code.
- [ ] Record baseline measurements.

## Phase 2 — Analyze

- [ ] Determine whether the bottleneck is CPU, I/O, memory, network, or contention.
- [ ] Check for N+1 queries, blocking calls, excessive serialization, or lock contention.
- [ ] Search `memories/` and project notes for similar optimizations.

## Phase 3 — Design the change

- [ ] Choose the smallest change that meets the target.
- [ ] Consider the trade-offs: complexity, maintainability, operational cost.
- [ ] Avoid premature optimization of code that is not on the critical path.

## Phase 4 — Implement & validate

- [ ] Make the change.
- [ ] Re-measure under the same conditions.
- [ ] Confirm the improvement is real and not a measurement artifact.
- [ ] Run correctness tests to ensure optimization didn't break behavior.

## Phase 5 — Document

- [ ] Record the baseline, change, and result.
- [ ] Update `architecture/<slug>/` if the optimization changed structure or deployment.
- [ ] Create/update memory notes for reusable optimization knowledge.
- [ ] ADR only if the optimization required a consequential architectural trade-off.

## Exit criteria

- [ ] Bottleneck is identified with evidence.
- [ ] Improvement is measured and meets the target.
- [ ] Correctness is preserved.
- [ ] Knowledge is preserved if reusable.
