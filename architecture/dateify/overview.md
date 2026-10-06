---
project: dateify
title: Dateify — Architecture Overview
updated: 2026-10-06
status: current
---

# Dateify — Architecture Overview

## System overview

Dateify is an AI-enhanced dating and connection platform featuring biometric identity verification, matchmaking, and simulated conversation flows. 

The system employs a client-server architecture:
1. **Compose Multiplatform Client (`dateify-mobile`)**: A single Kotlin codebase targeting Android, iOS, Desktop, and Wasm Web (hosted on Vercel at `https://web.dateify.app`).
2. **Rust Backend (`dateify-backend`)**: An Actix-web service handling identity, profile metadata, biometric verification, chat messaging, and matchmaking.
3. **Neon Serverless PostgreSQL**: Relational datastore augmented with `pgvector` for biometric face embedding storage and similarity indexing.
4. **AWS S3 (`dateify-prod-photos`)**: High-durability object storage for user profile photos and verification selfie frames.

```mermaid
graph TD
    subgraph Client Layer [Compose Multiplatform]
        WasmApp[Wasm Browser Client - web.dateify.app]
        AndroidApp[Android Client]
        iOSApp[iOS Client]
        Coil3[Coil 3 Async Image Loader + Ktor 3 Engine]
        
        WasmApp --> Coil3
        AndroidApp --> Coil3
        iOSApp --> Coil3
    end

    subgraph Hosting & Edge
        Vercel[Vercel Global CDN: web.dateify.app]
        Vercel --> WasmApp
    end

    subgraph Backend Services [Rust Actix-web]
        API[Rust Actix-web API: api.dateify.app]
        AuthRouter[/api/v1/auth]
        UserRouter[/api/v1/users]
        VerifyRouter[/api/v1/verification]
        MatchRouter[/api/v1/matches]
        ChatRouter[/api/v1/chat]

        API --> AuthRouter
        API --> UserRouter
        API --> VerifyRouter
        API --> MatchRouter
        API --> ChatRouter
    end

    subgraph Storage & Persistence
        NeonDB[(Neon PostgreSQL + pgvector)]
        S3[(AWS S3: dateify-prod-photos)]
    end

    ClientLayer -->|HTTPS REST with Bearer JWT| API
    Coil3 -->|Async Image Fetch| S3
    Coil3 -->|Authenticated Photo Fetch| VerifyRouter

    API -->|SQLx Queries & Vector Search| NeonDB
    API -->|Photo Uploads & Presigned URLs| S3
```

## Technology stack

- **Mobile & Web Client**: Kotlin 2.1.0, Compose Multiplatform 1.7.3.
- **Async Image Loading**: Coil 3 (`3.0.4`) with `coil-compose` and `coil-network-ktor3:3.0.4` engine (see [ADR-0011](../../adr/0011-coil-3-multiplatform-async-image-loading.md)).
- **UI & Design System**: Material 3, custom floating liquid-glass navigation bar (`DateifyTabRow`), 4:3 aspect-ratio match cards with slim 2dp photo borders (`DateifyCards`), typography and theme palettes.
- **Networking (Client)**: Ktor Client 3.x with content negotiation and Kotlinx Serialization.
- **Backend**: Rust (Actix-web 4, SQLx 0.8, Tokio, reqwest, jsonwebtoken, serde).
- **Database**: Neon Serverless PostgreSQL with `pgvector` extension for facial embedding similarity queries.
- **Object Storage**: AWS S3 (`dateify-prod-photos`).
- **Web Hosting**: Vercel Static Hosting (Wasm distribution).

## Components

### 1. `dateify-mobile` (Compose Multiplatform)
Shared Kotlin code located in `composeApp/src/commonMain/kotlin/app/dateify/mobile/`:
- **Core Components & Design**:
  - `DateifyAsyncImage`: Universal image loading wrapper utilizing Coil 3, providing fallback initials badge, loading shimmer/placeholder, and cross-platform error handling.
  - `DateifyCards`: Modern match card layout with 4:3 photo ratio, 2dp card border spacing, edge-to-edge imagery, floating action pills (Like, Pass, Superlike), and demographic overlay.
  - `DateifyTabRow`: Liquid-glass navigation bar simulating optical refraction with subtle translucent borders and active pill indicators.
- **Feature Screens**:
  - `DiscoverScreen`: Profile browsing and matchmaking interaction.
  - `MatchesScreen`: Active matches grid and mutual connection list.
  - `ChatsScreen` & `ChatDetailScreen`: Conversation threads with real personas and simulated dialogues.
  - `SimulationScreen`: AI dating simulation and scenario testing.
  - `VerificationScreen`: Biometric liveness selfie capture and onboarding.

### 2. `dateify-backend` (Rust Actix-web)
Monolithic service located in `backend/src/`:
- **`routes/users.rs`**: Profile creation, `/me` profile retrieval, and public demographic lookups (`/public/{id}`) returning age, city, bio, occupation, interests, and S3 photo URLs.
- **`routes/verification.rs`**: Facial liveness challenge evaluation, selfie frame ingestion, biometric embedding extraction, and authenticated photo proxy (`/photo/{filename}`) for secure image access.
- **`routes/matches.rs`**: Matchmaking feed generation, swipe telemetry, and mutual match links.
- **`routes/chat.rs`**: Messaging persistence, history retrieval, and simulated companion responses.

### 3. Neon PostgreSQL (`pgvector`)
- `users`: Core profile attributes (UUID, phone, email, name, age, city, bio, occupation, interests, verification status).
- `photos`: Media metadata, storage keys (`photos_of_A/...`), and 1536-dimensional or facial vector embeddings (`vector` column).
- `matches`: Pairwise user connections with mutual match timestamps.
- `messages`: Chronological conversation logs between users.

### 4. AWS S3 (`dateify-prod-photos`)
- Stores user profile photos (`photos_of_A/<uuid>-<filename>.jpg`) and private verification selfie frames.

## Service boundaries

- **Client-to-Backend**: The CMP client interacts solely via HTTPS REST APIs using Bearer JWT authentication tokens.
- **S3 Access Control**: S3 credentials (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`) reside exclusively in backend environment configurations. The client accesses images either via public S3 object URLs or authenticated backend streaming proxies (`/api/v1/verification/photo/{filename}`).
- **Zero Client Mock Assets**: All demo and production profiles (including demo partner personas like Maya) exist as real records in Neon DB and images in S3 (see [ADR-0012](../../adr/0012-persisted-users-and-s3-object-storage-over-client-mock-assets.md)).

## Data flow

### 1. Profile Discovery & Image Fetching
1. Client requests match feed from `GET /api/v1/matches/feed`.
2. Backend queries Neon DB for compatible profiles and associated photo records.
3. Backend returns JSON payload containing user details and remote photo URLs.
4. Client's `DateifyAsyncImage` hands URLs to Coil 3.
5. Coil 3 checks memory cache; on miss, it uses Ktor 3 engine to fetch bytes from AWS S3, decodes them to platform image bitmaps, and caches them in memory.

### 2. Biometric Verification & Facial Embedding Matching
1. Client captures liveness selfie frames during onboarding.
2. Client uploads frames via `POST /api/v1/verification/submit`.
3. Backend extracts facial feature vectors and compares them with registered profile photos using `pgvector` cosine similarity (`<=>`).
4. Backend uploads verified frames to AWS S3, sets user `verification_status = 'verified'` in Neon DB, and returns status to client.

## Deployment & operations

- **Wasm Web Distribution**:
  - Build command: `./gradlew :composeApp:wasmJsBrowserDistribution`
  - Output directory: `composeApp/build/dist/wasmJs/productionExecutable`
  - Deploy command: `vercel deploy --prod --yes`
  - Live Domain: `https://web.dateify.app`
- **Mobile Builds**:
  - Android: `./gradlew :composeApp:assembleRelease`
  - iOS: Gradle Xcode integration via CocoaPods/SPM framework packaging.
- **Backend Service**:
  - Build: `cargo build --release`
  - Runs with systemd or containerized runner listening on port 8080 behind reverse proxy.

## References

- [ADR-0011: Compose Multiplatform Async Image Pipeline with Coil 3](../../adr/0011-coil-3-multiplatform-async-image-loading.md)
- [ADR-0012: Persisted User Personas and S3 Object Storage over Client-Side Mock Assets](../../adr/0012-persisted-users-and-s3-object-storage-over-client-mock-assets.md)
- Project Memory: `[[dateify]]`
