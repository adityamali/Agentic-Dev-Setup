---
id: ADR-0010
title: Implement notes and attachments as platform capabilities
status: accepted
date: 2026-08-16
project: netgarage-platform
related: [ADR-0008, ADR-0009]
---

# ADR-0010: Implement notes and attachments as platform capabilities

## Context

The CRM master plan originally scoped Feature 3.1 ("Notes & attachments on any entity") as a CRM-only capability: a `crm.notes` table and reuse of the platform presign upload endpoint. CRM already consumes several platform modules (audit, files, RBAC), and the shared `EntityView` engine in `packages/ui` is intended to render any entity type without hardcoded CRM layouts.

Meanwhile, every other domain application in the suite (HR, Inventory, Support, Accounting, etc.) will need the same contextual notes and file-attachment behavior on its own records. Building a separate notes/attachments store per domain would duplicate schema, API surface, UI components, and permission models.

## Problem

Should notes and file attachments be owned by the CRM domain, or should they be platform capabilities that any domain can attach to any entity via a polymorphic `(entity_type, entity_id)` key?

## Decision

We will implement notes and attachments as **platform capabilities** in the existing Rust monolith, not as CRM-only tables.

- A new `platform.notes` table in `tenant_db` stores free-text notes keyed by `(tenant_id, entity_type, entity_id)`.
- A new `platform.files` table in `tenant_db` stores file metadata (today the files module only generates presigned URLs and logs metadata).
- A new `platform.entity_attachments` table in `tenant_db` links uploaded files to any `(entity_type, entity_id)` pair.
- New REST endpoints live under `/api/v1/platform/notes` and `/api/v1/platform/attachments`.
- The existing `/api/v1/platform/files/presign` endpoint will persist file metadata in `platform.files` before returning the upload URL.
- The shared `EntityView` engine in `packages/ui` will consume these platform endpoints in the `notes` and `files` sections, making the capability available to CRM immediately and to any future app that uses `EntityView`.

## Alternatives considered

### Alternative A — CRM-only notes/attachments table
- **Description:** Keep the original plan: create `crm.notes` and rely on the existing `/api/v1/platform/files/presign` mock endpoint for attachments.
- **Why rejected:** Every other domain would eventually need its own notes/attachments table, duplicating schema, migrations, handlers, permissions, and UI. It also conflicts with the goal of keeping `EntityView` generic and platform-driven.

### Alternative B — Separate platform file metadata, but CRM-only notes
- **Description:** Persist files in a platform table but keep notes in `crm.notes`.
- **Why rejected:** Notes and attachments are almost always rendered together in an entity timeline. Splitting ownership would force the timeline aggregator to union CRM and platform tables, complicating both queries and permissions.

### Alternative C — Store notes/attachments in `platform_db` instead of `tenant_db`
- **Description:** Place the new tables in the global `platform_db` and enforce tenant isolation via query filters instead of RLS.
- **Why rejected:** The project already uses `tenant_db` with Postgres RLS for all tenant-scoped data. Adding tenant-scoped tables to `platform_db` would break the established isolation model and require different authorization logic.

## Consequences

### Positive
- One set of tables, endpoints, permissions, and UI sections serves every domain application.
- CRM, HR, Inventory, Support, and future apps get notes/attachments "for free" once they adopt `EntityView`.
- The platform files module becomes stateful and can support download URLs, attachment lists, and storage-provider abstraction.
- Timeline aggregation (Feature 3.3) reads from a single platform notes source instead of a CRM-specific one.

### Negative / costs
- The change crosses the CRM/platform boundary, so it requires new platform permissions (`platform.note.*`, `platform.attachment.*`) and updates to CRM default roles.
- The platform cannot validate that `(entity_type, entity_id)` refers to a real record in a domain table; apps must ensure they only expose the section for entities the user can view.
- The existing files module must be extended to persist metadata, which slightly expands its scope.
- Any future app that wants notes/attachments must still decide which permissions to map to its roles.

## References

- CRM master plan: `docs/plans/2026-08-16-crm-master-plan.md`
- Workflow for architecture changes: `~/.agents/workflows/architecture-change.md`
- Workflow for implementing features: `~/.agents/workflows/implement-feature.md`
- Architecture overview: `~/.agents/architecture/netgarage-platform/overview.md`
