---
id: ADR-0011
title: Compose Multiplatform async image pipeline with Coil 3
status: accepted
date: 2026-10-06
project: dateify
related: [ADR-0012]
---

# ADR-0011: Compose Multiplatform async image pipeline with Coil 3

## Context

Dateify is a multiplatform client targeting Android, iOS, Desktop, and Web via Kotlin/Wasm (`wasmJs`). Dating applications are image-heavy, requiring asynchronous downloading, memory caching, decoding, and fallback handling for remote profile photographs.

In single-platform Android development, Coil 2 or Glide are standard. However, Compose Multiplatform (CMP) across desktop, iOS, and particularly WasmJs requires an image loading engine that can execute across all these targets without platform-specific JNI/NDK bindings or JVM threading assumptions. Early prototypes either relied on bundled client assets or lacked asynchronous remote image streaming on WasmJs.

## Problem

How should Dateify handle cross-platform asynchronous image loading, memory caching, and rendering across Android, iOS, Desktop, and WasmJs in Compose Multiplatform?

## Decision

We will standardize on **Coil 3** (`io.coil-kt.coil3:coil-compose:3.0.4`) paired with the **Ktor 3 Network Fetcher** (`io.coil-kt.coil3:coil-network-ktor3:3.0.4`) across all targets in `commonMain`.

- Replace any local or ad-hoc image rendering with a unified `DateifyAsyncImage` composable in `composeApp/src/commonMain/kotlin/app/dateify/mobile/components/DateifyAsyncImage.kt`.
- Use Coil's `AsyncImage` internally with a configured crossfade, error fallback, and fallback placeholder (initial-letter badge).
- Manage Coil dependencies via Gradle version catalog `libs.versions.toml` (`coil = "3.0.4"`).
- Use `coil-network-ktor3` to share Ktor's non-blocking coroutine networking stack on WasmJs and native iOS instead of platform-specific HTTP engines.

## Alternatives considered

### Alternative A — Kamel (Compose Multiplatform image loading library)
- **Description:** Use Kamel (`media.kamel:kamel-image`), an earlier multiplatform image loading library for CMP.
- **Why rejected:** Kamel has slower maintenance cycles, fewer built-in transformations and disk caching controls, and weaker ecosystem momentum compared to Coil 3, which is backed by the official Coil team and widely adopted for Compose Multiplatform.

### Alternative B — Expect/actual platform implementations
- **Description:** Implement custom `expect`/`actual` loaders (e.g. Coil 2 on Android, SDWebImage/Kingfisher on iOS, browser `Image()` on WasmJs).
- **Why rejected:** Introduces substantial duplicated bridge code across 4 targets, distinct memory caching semantics, inconsistent placeholder lifecycles, and high maintenance overhead.

### Alternative C — Raw Ktor client byte downloads into ImageBitmap
- **Description:** Download raw byte arrays using Ktor HTTP client and convert to Compose `ImageBitmap`.
- **Why rejected:** Reinvents memory cache eviction, LRU sizing, request deduplication, cancellation on scroll, and decoding pipelines from scratch.

## Consequences

### Positive
- A single `DateifyAsyncImage` component handles image fetching, caching, and fallback across Android, iOS, and WasmJs.
- Compatible with WasmJs browser execution on `web.dateify.app`.
- Automatic memory caching and request cancellation when cards or chat lists scroll out of view.
- Centralized fallback avatar UI (displaying name initials in a stylized badge) if remote image URLs are absent or network requests fail.

### Negative / costs
- Additional dependencies in `libs.versions.toml` (`coil-compose`, `coil-network-ktor3`).
- Requires strict network fetcher alignment with Ktor 3.x; version mismatches between Ktor and Coil could cause classpath conflicts during upgrades.
- WasmJs browser CORS rules still apply to images fetched directly from S3 (mitigated by configuring S3 CORS or proxying via backend).

## References

- Architecture Overview: [dateify/overview.md](../architecture/dateify/overview.md)
- Related ADR: [ADR-0012](0012-persisted-users-and-s3-object-storage-over-client-mock-assets.md)
- Technology Memory: `[[coil-3-compose-multiplatform]]`
