---
id: ADR-0009
title: Universal topbar and subordinate sidebar layout with hash sync
status: accepted
date: 2026-08-16
project: netgarage-platform
superseded_by:
related: [ADR-0008]
---

# ADR-0009: Universal topbar and subordinate sidebar layout with hash sync

## Context

Enterprise applications in the NetGarage suite require a consistent header navigation across all micro-frontends (including 9-dot suite app switching, search trigger, notifications, and profile management) alongside app-specific sidebars. Placing sidebars spanning the full viewport height alongside fragmented per-page topbars created visual inconsistency and rounded corner alignment flaws. Additionally, sidebar navigation items sharing the same base route (e.g. `/organization` and `/organization#roles`) caused duplicate active highlights.

## Problem

How should the universal suite topbar and app-specific sidebars be layered, and how should navigation state be tracked across path and hash fragment links?

## Decision

We will standardize on a two-tier layout hierarchy across all suite applications:
1. **Full-width rectangular Topbar**: The `Topbar` component is placed at the root of the app shell spanning 100% viewport width with flat, flush edges (no outer rounded borders).
2. **Subordinate Sidebar below Topbar**: App sidebars (`AppSidebar`) sit directly underneath the Topbar (`top-16` height boundary).
3. **Hover-Activated Morphing Collapse Toggle**: The sidebar header icon smoothly transforms into the collapse/expand toggle (`PanelLeftClose` / `PanelLeftOpen`) whenever the sidebar is hovered (via `group-hover/sidebar`), avoiding unnecessary persistent buttons.
4. **Hash-Aware Active Link State**: `AppSidebar` tracks `window.location.hash` alongside `usePathname()`, ensuring only the exact matching hash route or default base route receives the active indicator.

## Alternatives considered

### Alternative A — Full-height sidebar with right-hand Topbar
- Description: Let the sidebar run from top to bottom on the left, with Topbar starting to the right of the sidebar.
- Why rejected: Causes fragmented headers across different apps, breaks universal suite switching positioning, and leads to awkward nested border radiuses.

### Alternative B — Separate full pages for every sub-section
- Description: Create standalone Next.js routes for sub-views (e.g. `/roles` instead of `/organization#roles`).
- Why rejected: Multi-tab organization management benefits from instantaneous client-side tab transitions while retaining deep-linkable URLs.

## Consequences

### Positive
- Consistent Suite branding: Topbar is uniform across all satellite micro-frontends.
- Clean visual hierarchy: Seamless rectangular header with sidebar and content viewports cleanly divided below.
- Accurate active navigation: No multiple active state conflicts on multi-tab pages.

### Negative / costs
- Page layouts must wrap inside the `SettingsShell` or corresponding app shell rather than re-declaring per-page Topbars.

## References

- [[architecture/netgarage-platform/overview.md]]
- [[memories/projects/netgarage-platform.md]]
