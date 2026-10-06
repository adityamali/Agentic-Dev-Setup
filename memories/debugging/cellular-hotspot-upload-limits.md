---
type: debugging
title: Cellular Tethering TCP Upload Resets & MTU Drop on Large Payloads
tags: [debugging, network, cellular, ssh, tls]
created: 2026-10-06
updated: 2026-10-06
status: active
project: dateify
---

# Cellular Tethering TCP Upload Resets & MTU Drop on Large Payloads

## Symptom

When working over cellular hotspot tethering (specifically Jio or Airtel India 4G/5G connections):
1. Git pushes over SSH hang indefinitely or terminate with:
   ```
   Connection reset by 140.82.112.4 port 22
   fatal: the remote end hung up unexpectedly
   ```
2. HTTP POST or PUT requests containing large payloads (> 35-40 KB, such as embedded Base64 image strings or large multipart files) fail abruptly with:
   - `curl: (35) error:0A000438:SSL routines::tlsv1 alert internal error`
   - `curl: (35) LibreSSL SSL_connect: Connection reset by peer`
   - `HTTP/1.1 502 Bad Gateway` or connection drop during TLS handshake / frame transfer.

## Reproduction

1. Connect laptop to mobile hotspot (e.g. Jio 5G).
2. Attempt `git push origin main` on a branch containing a commit with large files over standard SSH `git@github.com:...` (port 22).
3. Attempt a `curl -X POST` with a 50KB JSON body containing an inlined Base64 image to an external HTTPS API.

## Hypotheses

| Hypothesis | Test | Result |
|------------|------|--------|
| Git remote server issue | Push from home Wi-Fi or wired connection | Works immediately. Ruled out server outage. |
| Port 22 throttling / middlebox RST | Configure SSH to route over port 443 via `ssh.github.com` | Pushes succeed reliably. Confirmed cellular middlebox filters port 22. |
| Middlebox MTU / MSS clamping breaking fragmented TLS records | Send payload in chunks < 30KB vs single monolithic 100KB payload | Chunked/small payloads succeed; monolithic payload triggers TCP RST. |

## Root cause

Indian cellular carriers (Jio, Airtel) deploy Carrier-Grade NAT (CGNAT) and Deep Packet Inspection (DPI) middleboxes with aggressive MTU clamping (often 1380-1420 bytes) and strict connection state tracking:
1. **SSH Port 22 Interference**: Middleboxes inspect or throttle non-standard high-throughput SSH connections over port 22, injecting TCP RST packets when packet bursts exceed buffer limits.
2. **TLS Packet Size / Fragmentation**: Monolithic TLS upload bursts with large records (> 16KB unchunked) get fragmented across lower MTU links, triggering packet drops or bad record MAC alerts when middleboxes drop fragmented TCP segments.

## Fix

1. **SSH over HTTPS Port 443**:
   Configure `~/.ssh/config` to route GitHub traffic through port 443:
   ```ssh-config
   Host github.com
       Hostname ssh.github.com
       Port 443
       User git
   ```
2. **Eliminate Large Inlined Payloads**:
   Remove inlined Base64 image assets from source code and JSON API payloads (see [[dateify]], ADR-0012).
3. **Stream Media Directly to Object Storage**:
   Upload photos directly to AWS S3 using presigned URLs with chunked streaming rather than passing multi-megabyte payloads through monolithic backend JSON endpoints.

## Prevention

- Never embed binary images as Base64 strings in application code or Git repositories.
- Use S3 direct uploads / presigned URLs for media.
- Keep `~/.ssh/config` permanently configured with `Hostname ssh.github.com` and `Port 443`.

## Related

- [[dateify]]
