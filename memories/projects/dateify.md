---
type: project
title: Dateify — AI Dating & Verification Platform
tags: [dateify, kotlin-multiplatform, compose-multiplatform, rust, actix-web, neon-postgres, pgvector, aws-s3, wasm]
created: 2026-10-06
updated: 2026-10-06
status: active
project: dateify
---

# Dateify — AI Dating & Verification Platform

Dateify is an AI-enhanced dating platform featuring biometric face verification, real-time matchmaking, and simulated conversation flows.

## Current state

- **Mobile & Web Client (`dateify-mobile`)**: Compose Multiplatform running on Kotlin 2.1.0 and CMP 1.7.3.
  - Active targets: Android, iOS, and Wasm Browser (Web).
  - Web client deployed live to Vercel at `https://web.dateify.app`.
  - UI system: Material 3, floating optical liquid-glass bottom bar (`DateifyTabRow`), 4:3 ratio match cards with 2dp borders and edge-to-edge images (`DateifyCards`).
  - Unified async image loading pipeline using Coil 3 (`3.0.4`) and Ktor 3 engine (`DateifyAsyncImage`).
  - Zero hardcoded mock persona assets; all user data is retrieved via backend APIs.
- **Backend Service (`dateify-backend`)**: Rust Actix-web server connecting to Neon Serverless PostgreSQL and AWS S3.
  - User and demographic endpoints (`/api/v1/users/me`, `/api/v1/users/public/{id}`).
  - Biometric liveness selfie capture and verification with `pgvector` facial embedding search.
  - Authenticated photo proxy endpoint (`/api/v1/verification/photo/{filename}`) for secure image streaming.
  - Real companion personas (e.g. Maya `e5a32c10-9b87-4d6e-a1f2-7c8d9e0f1a2b`) persisted in Neon DB with photos stored in AWS S3 (`dateify-prod-photos`).

## Key facts & conventions

- **Card Layout**: Profile photos on match cards use a strict 4:3 aspect ratio (height:width = 4:3, i.e. `aspectRatio(3f / 4f)`) with a 2dp border margin to card edges.
- **Image Pipeline**: Always use `DateifyAsyncImage` rather than raw image composables or platform loaders. Handles initial badges, loading states, and remote errors automatically.
- **No Client Mocks**: Companion personas and seed profiles are stored in the database, never hardcoded in client source or resources.
- **Cellular Network Constraint**: When developing on mobile tethering/hotspots in India (Jio/Airtel), avoid pushing large git blobs over port 22 or sending large HTTP payloads (> 35KB) without chunking.

## Decisions that matter

- ADR-0011: Compose Multiplatform async image pipeline with Coil 3
- ADR-0012: Persist user personas and S3 object storage over client-side mock assets

## Technologies in use

- [[coil-3-compose-multiplatform]]
- Kotlin Multiplatform / Compose Multiplatform
- Rust Actix-web & SQLx
- Neon PostgreSQL with `pgvector`
- AWS S3
- Vercel Web Hosting

## Related

- [[cellular-hotspot-upload-limits]]
- [[coil-3-compose-multiplatform]]
