---
type: technology
title: Coil 3 in Compose Multiplatform
tags: [coil, compose-multiplatform, kotlin, wasm, android, ios]
created: 2026-10-06
updated: 2026-10-06
status: active
---

# Coil 3 in Compose Multiplatform

## What it is

Coil 3 (`io.coil-kt.coil3`) is the multiplatform rewrite of Coil, an asynchronous image loading and caching library for Kotlin and Compose Multiplatform (supporting Android, JVM/Desktop, iOS, and WasmJs).

## Why we use it

Compose Multiplatform lacks a built-in cross-platform asynchronous image loader. Before Coil 3, teams either used Kamel or wrote tedious `expect`/`actual` bridges per platform. Coil 3 provides:
- First-class multiplatform support including Kotlin/Wasm (`wasmJs`).
- Decoupled network fetchers: `coil-network-ktor3` enables sharing the existing Ktor 3 HTTP client engine across Android, iOS, and Wasm without duplicating network stacks.
- Automatic memory caching, bitmap pooling, request deduplication, and scroll-aware cancellation.

## Key patterns

### 1. Version catalog setup (`libs.versions.toml`)
```toml
[versions]
coil = "3.0.4"

[libraries]
coil-compose = { module = "io.coil-kt.coil3:coil-compose", version.ref = "coil" }
coil-network-ktor3 = { module = "io.coil-kt.coil3:coil-network-ktor3", version.ref = "coil" }
```

### 2. Unified Wrapper (`DateifyAsyncImage`)
Wrap Coil's `AsyncImage` in a project-standard composable that supplies fallback initial badges, loading indicators, and error recovery:
```kotlin
@Composable
fun DateifyAsyncImage(
    model: Any?,
    contentDescription: String?,
    modifier: Modifier = Modifier,
    contentScale: ContentScale = ContentScale.Crop,
    fallbackText: String = ""
) {
    var hasError by remember(model) { mutableStateOf(false) }
    
    if (model == null || hasError) {
        AvatarInitialBadge(text = fallbackText, modifier = modifier)
    } else {
        AsyncImage(
            model = model,
            contentDescription = contentDescription,
            modifier = modifier,
            contentScale = contentScale,
            onError = { hasError = true }
        )
    }
}
```

## Gotchas

- **Ktor Version Alignment**: Ensure Coil's network module matches the Ktor major version (e.g. `coil-network-ktor3` for Ktor 3.x; using `coil-network-ktor2` will throw runtime `ClassNotFoundException` or linkage errors).
- **WasmJs CORS**: In the browser, Coil's HTTP requests are subject to browser CORS policies. Image servers (like S3 buckets) must either send appropriate `Access-Control-Allow-Origin` headers or be served through a backend proxy.
- **Skiko / WebGL Context on Wasm**: Heavy concurrent image decoding on WasmJs can stutter the UI thread if images are enormous. Request downscaled images or thumbnails where possible.

## Alternatives considered

- **Kamel**: Earlier community library, but less active maintenance and fewer built-in caching utilities compared to Coil 3.
- **Platform-specific loaders**: Coil 2 on Android, SDWebImage on iOS, HTML `<img>` on Web. Rejected due to code duplication and disjointed composable lifecycles.

## Related

- [[dateify]]
