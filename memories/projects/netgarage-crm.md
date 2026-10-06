---
project: netgarage-platform
domain: crm
created: 2026-08-16
---

# NetGarage CRM — Project Memory

## Platform notes & attachments (Feature 3.1)

Re-scoped from a CRM-only `crm.notes` table to a platform-wide capability.

- Tables live in `tenant_db.platform` schema: `platform.notes`, `platform.files`, `platform.entity_attachments`.
- Endpoints are under `/api/v1/platform/notes` and `/api/v1/platform/attachments`.
- The platform does **not** validate that `(entity_type, entity_id)` points to a real domain record. Apps own entity existence; the platform owns the note/attachment lifecycle.
- The existing `/api/v1/platform/files/presign` endpoint now persists file metadata in `platform.files` before returning the upload URL.
- Permissions are platform-level: `platform.note.{view,create,update,delete}` and `platform.attachment.{view,create,delete}`. CRM default roles (CRM Admin, Sales Manager, Sales Rep) include them.
- `EntityNotes` and `EntityFiles` in `packages/ui/src/entity-view/` call the platform endpoints using `entity_type`/`entity_id`, so any app using `EntityView` gets the capability.

## Validation commands

- `cargo check` / `cargo build` in `backend/`
- `pnpm --filter crm build` for the frontend
- Smoke test: register tenant → login → `POST /api/v1/platform/notes`, `POST /api/v1/platform/files/presign`, `POST /api/v1/platform/attachments`

## Gotchas

- `AuthMiddleware` falls back to a dev user for missing/invalid tokens, but `require_permission` requires `auth_user.user_id` to be a UUID. Use a real login token for RBAC-protected endpoints.
- Files presign previously only logged metadata. It now writes to `platform.files`, so the migration must be applied before uploads work.
