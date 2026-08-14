---
project: <slug>
title: <Project> — Architecture
updated: YYYY-MM-DD
status: draft
---

# <Project> — Architecture

<!-- One file per concern, or a single overview for small systems. See
     ../architecture/README.md for the full doc-set and update triggers.
     For a large system, split into overview.md / components.md / dataflow.md /
     deployment.md within architecture/<slug>/ and keep this as the index. -->

## System overview

<!-- What the system does, who it serves, and the one-paragraph shape of it.
     A diagram (ASCII or linked) belongs here. -->

## Technology stack

<!-- Languages, frameworks, datastores, infra — with versions that matter and
     the ADR that justifies any non-obvious choice. -->

## Components

<!-- Each deployable/logical unit: its responsibility, its boundary, its owner.
     Link to per-component docs as they grow. -->

## Service boundaries

<!-- Where one component ends and another begins; the contracts between them
     (per AR-03). What is explicitly NOT shared. -->

## Dependencies

<!-- Internal: which component depends on which (direction matters, per AR-01).
     External: third-party services, libraries that shape the design. -->

## Data flow

<!-- How data moves: producers, transformations, consumers, stores. Trace the
     important flows end to end (per AR-07). -->

## Shared modules

<!-- Code shared across components, why it's shared, and how changes to it are
     governed (per AR-06). -->

## External integrations

<!-- Third-party APIs, webhooks, auth providers — with their failure behavior
     (per AR-08) and any security notes (SC-*). -->

## Deployment

<!-- Environments, how it's built/shipped, how to roll back. Link runbooks. -->

## Open questions

<!-- Known unknowns and decisions not yet made — candidates for future ADRs. -->
