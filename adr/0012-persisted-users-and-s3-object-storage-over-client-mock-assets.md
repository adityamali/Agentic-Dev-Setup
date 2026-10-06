---
id: ADR-0012
title: Persist user personas and S3 object storage over client-side mock assets
status: accepted
date: 2026-10-06
project: dateify
related: [ADR-0011]
---

# ADR-0012: Persist user personas and S3 object storage over client-side mock assets

## Context

During early mobile prototyping of the Dateify client, demo companion personas (such as Maya) were bundled directly into the Kotlin application code as static assets, drawables, or hardcoded Base64 strings.

This approach exhibited several critical flaws:
1. **Application Bundle Bloat**: Embedding high-resolution portrait assets in the client binary inflated the compiled Wasm and Android APK bundle sizes significantly.
2. **Network / MTU Failures on Cellular Hotspots**: Transferring commits or local testing over mobile tethering hit TCP RSTs and bad record MAC failures when large Base64 blobs were committed into code.
3. **Database Architecture Deviation**: Mock personas lacked real relational records in Neon PostgreSQL (`users`, `matches`, `messages`, `photos` with pgvector embeddings). They could not participate in real backend matchmaking, chat synchronization, or verification algorithms.
4. **Architectural Divergence**: Client code had branched logic checking for hardcoded persona identifiers instead of uniformly consuming backend REST endpoints.

## Problem

Should demo and companion profiles be hardcoded as client-side mock assets or persisted as first-class database entities with remote object storage?

## Decision

We will completely eliminate client-side mock personas and assets, persisting all users as first-class rows in **Neon PostgreSQL** and storing their profile media in **AWS S3 (`dateify-prod-photos`)**.

- Delete all client-side embedded mock assets (such as `MayaPortraitAsset.kt`, `MayaPortrait*.kt`, and associated XML/drawable resources) from `dateify-mobile`.
- Provision Maya as a real user in Neon DB (`users` table with UUID `e5a32c10-9b87-4d6e-a1f2-7c8d9e0f1a2b`, age 24, Bangalore, Software Engineer, verified status).
- Upload Maya's high-resolution portrait to AWS S3 under `photos_of_A/e5a32c10-9b87-4d6e-a1f2-7c8d9e0f1a2b-maya.jpg`, compute facial embeddings, and insert a row in the `photos` table.
- Link mutual match and chat message records between the authenticated user and Maya directly in Neon DB.
- Expose complete demographic and photo attributes via backend endpoints `GET /api/v1/users/public/{id}` and `GET /api/v1/users/me`.
- Extend authenticated photo streaming endpoint `GET /api/v1/verification/photo/{filename}` in the Rust backend to securely serve or proxy profile photos.
- Ensure the client loads all profiles through `DateifyApiClient` and renders imagery via `DateifyAsyncImage` (ADR-0011).

## Alternatives considered

### Alternative A — Keep mock assets bundled in the client
- **Description:** Maintain bundled drawable/bitmap assets in `composeApp` resources for demo flows.
- **Why rejected:** Bloats the client bundle, prevents real multi-device testing, creates two parallel codepaths (one for mock users, one for real users), and triggers cellular MTU packet drops during development.

### Alternative B — Local JSON mock fixtures on client
- **Description:** Keep persona JSON metadata and Base64 images in client assets and load them via a mock repository.
- **Why rejected:** Still bloats the binary and fails to test database indexes, pgvector similarity queries, and real network latency.

### Alternative C — Seed mock personas in a local SQLite client database
- **Description:** Pre-populate a client-side SQLDelight/Room database on app start.
- **Why rejected:** Couples demo data to client release cycles and prevents server-driven profile updates, chat responses, or matchmaking changes.

## Consequences

### Positive
- Client binary size is minimized; compiled Wasm bundle loads significantly faster on `https://web.dateify.app`.
- Single uniform codepath: the mobile app treats all users identically via standard REST APIs.
- Backend facial verification (`pgvector`) and matching algorithms operate on authentic photo records and vector embeddings.
- Server-side persona updates (changing photos, bio, interests, location) reflect immediately across all clients without rebuilding the app.

### Negative / costs
- Backend and cloud database must be seeded with companion personas during environment setup.
- App requires network connectivity or explicit HTTP caching to view profile cards (handled by Coil 3 cache).
- AWS S3 bucket and Neon DB credentials are required for backend integration testing.

## References

- Architecture Overview: [dateify/overview.md](../architecture/dateify/overview.md)
- Related ADR: [ADR-0011](0011-coil-3-multiplatform-async-image-loading.md)
- Project Memory: `[[dateify]]`
- Debugging Memory: `[[cellular-hotspot-upload-limits]]`
