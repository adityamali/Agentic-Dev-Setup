---
id: WF-05
title: Database migration
---

# WF-05: Database migration

## Trigger

Changing schema, indexes, constraints, or data in a persisted store.

## Phase 1 — Plan the migration

- [ ] Understand the current schema and the target schema.
- [ ] Identify all queries, models, and code paths that touch the changed data.
- [ ] Determine if the change is backward-compatible or requires a phased rollout.
- [ ] For destructive changes, plan the data transformation and rollback path.
- [ ] Get user confirmation for risky or irreversible changes.

## Phase 2 — Safety checks

- [ ] Back up data before destructive operations.
- [ ] Estimate the runtime and lock behavior of the migration in production.
- [ ] For large tables, prefer online/schema-change tools or additive-then-subtractive patterns.
- [ ] Ensure migration scripts are idempotent where possible.

## Phase 3 — Implement

- [ ] Write the migration script.
- [ ] Update models, repositories, and queries to match the new schema.
- [ ] Add validation/constraints at the application layer where appropriate.
- [ ] Avoid dual-writes unless part of an explicit cutover plan.

## Phase 4 — Test

- [ ] Run the migration against a copy of production-like data.
- [ ] Test rollback.
- [ ] Verify all affected code paths still work.
- [ ] Add tests for new constraints or data invariants.

## Phase 5 — Document

- [ ] Document the migration steps and rollback procedure.
- [ ] Update `architecture/<slug>/` data model and deployment sections.
- [ ] Create an ADR for consequential schema or data-model decisions.
- [ ] Create/update memory notes for tricky migration patterns or gotchas.

## Exit criteria

- [ ] Migration is tested in a production-like environment.
- [ ] Rollback path is documented and tested.
- [ ] Application code is consistent with the new schema.
- [ ] ADR/memory hooks satisfied.
