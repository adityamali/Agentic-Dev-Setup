---
id: WF-04
title: Create API
---

# WF-04: Create API

## Trigger

Designing or implementing a new API: REST, RPC, GraphQL, WebSocket, CLI, library interface, or event schema.

## Phase 1 — Design

- [ ] Define the consumer and their use cases.
- [ ] Define the operations/resources/events.
- [ ] Choose an interface style appropriate to the project.
- [ ] Draft the contract: request/response shapes, status codes, error formats, rate limits.
- [ ] Review against existing APIs in the project for consistency.
- [ ] Get user confirmation for non-trivial contracts.

## Phase 2 — Validate the design

- [ ] Check for security: auth, authorization, input validation (SC-02, SC-10).
- [ ] Check for failure modes and error contracts (AR-08).
- [ ] Consider versioning and backward compatibility from day one.
- [ ] Confirm the design fits the deployment and client constraints.

## Phase 3 — Implement

- [ ] Implement the contract, not a superset (CQ-05).
- [ ] Validate and sanitize all inputs at the boundary (SC-02).
- [ ] Return consistent, useful errors (CQ-10).
- [ ] Add observability: logs, metrics, traces where appropriate.

## Phase 4 — Test

- [ ] Write contract tests for happy and error paths.
- [ ] Test input validation, auth failures, and rate limiting.
- [ ] Test serialization/deserialization round-trips.

## Phase 5 — Document

- [ ] Document the contract with examples (DOC-06).
- [ ] Update `architecture/<slug>/` for new endpoints/service boundaries.
- [ ] Create an ADR if the API design involved consequential choices (e.g., GraphQL vs REST, versioning strategy).
- [ ] Create/update memory notes for non-obvious API behavior or gotchas.

## Exit criteria

- [ ] Contract is documented and implemented consistently.
- [ ] Validation, auth, and error paths are tested.
- [ ] Architecture and ADR/memory hooks satisfied.
