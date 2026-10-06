---
id: ADR-0008
title: Multi-app monorepo with shared UI, auth, and API client packages
status: accepted
date: 2026-08-16
project: netgarage-platform
superseded_by:
related: []
---

# ADR-0008: Multi-app monorepo with shared UI, auth, and API client packages

## Context

The NetGarage Enterprise ecosystem consists of multiple distinct web applications (e.g., Launcher, Admin Settings, CRM, HRMS, Inventory, Analytics) interacting with a centralized Rust backend. Building individual applications without shared package boundaries leads to duplicated authentication contexts, inconsistent navigation, duplicated API models, and CSS design system drift.

## Problem

How should we structure the NetGarage frontend codebase to support independent micro-frontend satellite applications while maintaining a single cohesive design system, shared authentication state, and typed API clients?

## Decision

We will organize the frontend into a `pnpm` workspaces monorepo:
1. `packages/ui` (`@netgarage/ui`): Shared React 19 component library exposing `Topbar`, `AppSwitcher`, `AppSidebar`, `SidebarProvider`, `Logo`, `Button`, `Card`, `Badge`, and canonical CSS design tokens (`theme.css`).
2. `packages/auth` (`@netgarage/auth`): Shared authentication context provider (`AuthProvider`, `useAuth`) interfacing with Zitadel OIDC tokens.
3. `packages/api-client` (`@netgarage/api-client`): Shared TypeScript HTTP client and domain entity types (`Branch`, `Department`, `Team`, `User`, `Role`, `Permission`, `BillingPlan`, `WorkflowItem`, `AuditLogItem`, `FeatureFlag`).
4. `apps/*`: Independent Next.js satellite applications (`apps/launcher` on port 3000, `apps/settings` on port 3001) consuming the shared workspace packages.

## Alternatives considered

### Alternative A — Single monolithic Next.js application
- Description: Host all applications and administrative dashboards under one Next.js project using nested App Router directories.
- Why rejected: Causes slow build times, tight coupling of unrelated business domains, and prevents independent scaling or deployment of satellite applications.

### Alternative B — Separate isolated git repositories
- Description: Place each app in its own repository and publish shared libraries to an internal npm registry.
- Why rejected: High overhead for updating shared UI components during rapid active development and lack of atomic multi-package commits.

## Consequences

### Positive
- Component reuse: Changes to `Topbar`, `AppSwitcher`, or `AppSidebar` automatically propagate across all suite applications.
- Type safety: Backend API response types in `@netgarage/api-client` are shared across all consumer apps.
- Cohesive branding: Single source of truth for design tokens (`theme.css`) and vector brand assets (`Logo`).

### Negative / costs
- Monorepo tooling setup and pnpm filter scripts required for builds and dev servers.
- Need to keep package dependencies (e.g., React 19, Lucide React, Tailwind) aligned across workspaces.

## References

- [[architecture/netgarage-platform/overview.md]]
- [[memories/projects/netgarage-platform.md]]
