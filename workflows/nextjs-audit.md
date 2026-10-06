---
id: WF-10
title: Audit and fix Next.js project
---

# WF-10: Audit and fix Next.js project

## Trigger

A Next.js project needs a health check, build/type/lint failures must be resolved, imports need fixing, or the codebase is being brought up to current Next.js conventions.

## Preparation

- [ ] Load the [`nextjs`](../skills/nextjs/SKILL.md) skill.
- [ ] Read the project README and any `AGENTS.md` / architecture docs.
- [ ] Confirm the project root contains a `package.json` with `next` as a dependency.
- [ ] Determine the package manager from the lockfile (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `bun.lockb`).
- [ ] Determine whether the project uses App Router, Pages Router, or both.

## Phase 1 — Discovery

- [ ] Read `package.json`, `next.config.*`, `tsconfig.json` / `jsconfig.json`, and the top-level directory layout.
- [ ] Record Next.js and React versions.
- [ ] Identify existing scripts (`lint`, `type-check`, `build`, `test`).
- [ ] Note any `.env*` files (do not read values unless needed).
- [ ] List known symptoms from the user or from CI logs.

## Phase 2 — Diagnostics

Run the project's own quality gates in order and capture full output:

- [ ] Install dependencies if `node_modules` is missing or the lockfile is newer.
- [ ] Run `lint`.
- [ ] Run TypeScript: `tsc --noEmit` or the project's type-check script.
- [ ] Run `build`.
- [ ] Run `test` if a test script exists.

If a step fails, record the failure and continue collecting output when safe. Do not fix yet.

## Phase 3 — Classify

Map each failure to a bucket (per the Next.js skill):

- [ ] Imports / module resolution.
- [ ] TypeScript / type errors.
- [ ] `next.config.*` or framework configuration.
- [ ] Routing conflicts (App Router vs Pages Router, duplicate routes).
- [ ] Data fetching (deprecated APIs, async params, cache behavior).
- [ ] Server/client boundary mistakes (`'use client'`, SSR-only APIs).
- [ ] Images / fonts / metadata.
- [ ] Dependencies / peer dependencies.
- [ ] Lint / formatting.

State the root cause for each bucket before changing code.

## Phase 4 — Fix

Fix in order of dependency: imports → types → config → routing → data fetching → runtime → dependencies → lint.

- [ ] Resolve broken, missing, or dead imports. Correct relative paths and alias usage.
- [ ] Fix type errors without widening to `any`.
- [ ] Validate and correct `next.config.*` for the installed Next.js major version.
- [ ] Resolve routing conflicts and missing route files.
- [ ] Update data fetching to the router-appropriate pattern.
- [ ] Add or remove `'use client'` directives based on actual client-only needs.
- [ ] Fix image/font/metadata usage to match current Next.js APIs.
- [ ] Align React and other peer dependencies with Next.js requirements.
- [ ] Run lint/format fixes only after code changes are stable.

Stop and re-plan if a fix would change product behavior, require a dependency or architecture decision, or the scope grows beyond the audit.

## Phase 5 — Validation

- [ ] Re-run `lint`.
- [ ] Re-run TypeScript / type-check.
- [ ] Re-run `build`.
- [ ] Re-run `test` if present.
- [ ] Repeat classify-fix-validate until green or only documented exceptions remain.
- [ ] Confirm no unrelated files were modified.

## Phase 6 — Documentation

- [ ] Update project docs if behavior or setup steps changed (DOC-01).
- [ ] Update `architecture/<slug>/` if structure or routing changed.
- [ ] Create or update ADR if a consequential dependency or structural decision was made.
- [ ] Create or update memory notes for non-obvious gotchas (e.g., custom aliases, build workarounds, version-specific fixes).

## Exit criteria

- [ ] Diagnostics run and results captured.
- [ ] Root causes identified and fixed.
- [ ] `lint`, `type-check`, `build`, and `test` pass, or remaining failures are documented and intentional.
- [ ] Change set is minimal and reviewable.
- [ ] User is informed of what was fixed, what remains, and any follow-ups.
