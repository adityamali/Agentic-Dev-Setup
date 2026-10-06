---
type: project
title: NetGarage Platform & Multi-App Monorepo
tags: [rust, nextjs, postgresql, multi-tenancy, zitadel, pnpm-workspaces, design-system]
created: 2026-08-14
updated: 2026-08-16
status: active
project: netgarage-platform
---

# NetGarage Platform & Multi-App Monorepo

The centralized foundation layer and micro-frontend monorepo for the NetGarage Enterprise Suite.

## Domain applications

- [[netgarage-crm]] — CRM & Sales Hub (foundation complete; next: Organization CRUD)

## Current state

- **Monorepo Architecture**: Managed with `pnpm` workspaces.
  - `apps/launcher`: Suite launchpad (Port 3000).
  - `apps/settings`: Platform governance, org topology, RBAC, billing, workflows, audit logs, feature flags (Port 3001).
  - `packages/ui`: Shared UI component library, universal Topbar, AppSidebar, AppSwitcher, vector Logo, and `theme.css`.
  - `packages/auth`: Shared Zitadel OIDC authentication context.
  - `packages/api-client`: Shared TypeScript client and domain models.
- **Backend**: Rust Actix-web monolith with dual-database pool architecture (`platform` and `tenant` databases with Postgres RLS).

## Key facts & conventions

- **Universal Suite Layout**:
  - Full-width rectangular `Topbar` spans across 100% of the top without outer rounded borders.
  - `AppSidebar` sits directly beneath the Topbar (`top-16`).
  - The sidebar header icon morphs into `PanelLeftClose` / `PanelLeftOpen` collapse toggle on sidebar hover (`group-hover/sidebar`).
  - Active link highlights in `AppSidebar` are hash-aware (syncing `#roles` vs `/organization` seamlessly).
- **Design Tokens (`packages/ui/src/theme.css`)**:
  - Interactive primary colors: `--interactive-primary: #18181b`, `--interactive-primary-hover: #27272a`, `--interactive-primary-active: #09090b`.
  - Brand accents: `--brand-primary: #ff5810`.
  - Canvases: `--canvas-default: #ffffff`, `--canvas-subtle: #f5f5f7`.
- **Image Import Rule in TSX**:
  - Always explicitly import `Image` from `next/image` or use the embedded `Logo` SVG component from `@netgarage/ui` to prevent collision with the global DOM `window.Image` constructor.

## Related

- [[architecture/netgarage-platform/overview.md]]
- [[adr/0008-pnpm-workspaces-and-shared-packages-for-micro-frontend-suite.md]]
- [[adr/0009-universal-topbar-and-subordinate-sidebar-layout-with-hash-sync.md]]
