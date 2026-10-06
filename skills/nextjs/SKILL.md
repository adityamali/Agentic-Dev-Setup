---
name: nextjs
description: Audit, diagnose, and fix Next.js projects. Covers import resolution, type errors, build failures, routing issues, deprecated APIs, configuration drift, and common App Router / Pages Router mistakes. Load when working on a Next.js codebase that needs health checking, refactoring, or bug fixes.
version: 0.1.0
author: locally authored
compatible_agents: [claude, codex, opencode, cursor, gemini]
dependencies: []
tags: [nextjs, react, audit, frontend, build, typescript]
---

# Next.js skill

Audit, diagnose, and fix Next.js projects methodically. Prefer fixing root causes over papering over symptoms. Stop and ask when a fix would change product behavior.

## When to load this skill

Load when the user mentions any of:

- "audit my Next.js app", "health check", "fix my Next.js project"
- "broken imports", "missing imports", "import errors"
- "Next.js build fails", "build error", "compilation error"
- "migrate to App Router", "Pages Router issues", "route errors"
- "TypeScript errors in Next.js", "type errors"
- "deprecated Next.js API", `getInitialProps`, `getServerSideProps`, `<Link>` without `href`, `next/image` migration

Do not load for generic React work unless Next.js is the target framework.

## Purpose

Give the agent a repeatable way to bring a Next.js project from a broken or unknown state to a green build and lint/type-check pass, while preserving existing behavior.

## Prerequisites

- Node.js is installed and `node --version` / `npm --version` work.
- The project has `next` as a dependency (dev or prod) in `package.json`.
- A package manager is detectable: `npm`, `yarn`, `pnpm`, or `bun`. Prefer the project's lockfile over a global default.
- The agent has read/write access to the project root.

## Instructions

### 1. Orient yourself

Read in this order before changing code:

1. `package.json` — Next.js version, React version, scripts, dependencies, TypeScript presence.
2. `next.config.js` / `next.config.mjs` / `next.config.ts` — custom config, rewrites, redirects, images, output mode.
3. `tsconfig.json` / `jsconfig.json` — paths, module resolution, strictness.
4. `.env*` files (names only; do not read values unless needed for a fix).
5. Directory layout: `app/` vs `pages/` vs both. Determine the primary router.
6. `README.md` / `AGENTS.md` if present.

### 2. Detect the router and framework version

- App Router exists if `app/` contains `layout.tsx`, `layout.jsx`, `page.tsx`, `page.jsx`, or `loading/error/not-found` files.
- Pages Router exists if `pages/` contains `_app.*`, `_document.*`, or route files.
- Record the major Next.js version (14, 15, etc.) and React version. Some fixes differ between majors.

### 3. Run diagnostics and capture output

Run the project's own quality gates in this order, stopping after each to record failures:

1. `install` if `node_modules` is missing or lockfile changed.
2. The package manager's `lint` script (e.g., `npm run lint`).
3. TypeScript: `npx tsc --noEmit` (or the project's `type-check` script).
4. Build: the package manager's `build` script.
5. Tests: the package manager's `test` script, if one exists.

Capture full output. Use it as the issue list. Do not guess at issues before running diagnostics.

### 4. Classify issues

Map each failure to one of these buckets:

| Bucket | Typical signals |
|--------|-----------------|
| Import / module resolution | `Cannot find module`, `Cannot resolve`, `TS2307`, wrong relative path, missing extension, alias not recognized |
| Type / TSConfig | `TS2xxx` errors, strict-null mismatches, missing types, `skipLibCheck` issues |
| Config | Invalid `next.config.*`, unsupported keys for the installed Next.js version |
| Routing | `404`, conflicting routes, missing `page` export, wrong `layout` location, catch-all overlaps |
| Data fetching | Deprecated `getInitialProps`/`getServerSideProps` in App Router, wrong `fetch` cache behavior, `await` missing on `params`/`searchParams` in async components |
| Runtime / hooks | `use client` missing, `useEffect` in server component, `window`/`document` used during SSR |
| Images / fonts | `next/image` src type, `next/font` import path, missing `remotePatterns` |
| Dependencies | Peer-dep mismatch, missing package, wrong React/Next versions |
| Lint / formatting | ESLint/Prettier failures, unused vars, missing dependencies arrays |

Fix in order: imports → types → config → routing → data fetching → runtime → dependencies → lint.

### 5. Fix imports and module resolution

1. **Missing imports.** Add the missing import from the correct source. Prefer named exports. For third-party packages without types, add `@types/<pkg>` if available or a minimal `.d.ts` shim.
2. **Wrong relative paths.** Use the project's `tsconfig.json` `paths` aliases when available. Correct `../` drift caused by file moves.
3. **Extension-less imports.** In ESM or TypeScript, keep extension-less imports unless the project explicitly requires extensions. Do not add `.js` to TypeScript files unless `moduleResolution` is `nodenext`.
4. **Barrel files causing cycles.** If a circular import error appears, break the cycle by importing from the concrete module instead of the barrel.
5. **Case sensitivity.** Fix filename casing mismatches (common when a project moves between macOS and Linux/CI).
6. **Dead imports.** Remove unused imports when the linter flags them and removal is safe.

### 6. Fix type errors

1. Run `tsc --noEmit` after every batch of fixes.
2. Add explicit types only where inference fails. Do not widen types to `any` to silence errors.
3. For missing Next.js types, ensure `next` is installed (not just `next/head` imports).
4. For `params`/`searchParams` in async App Router components, type as `Promise<{ [key: string]: string | string[] | undefined }>` for `params`, and `Promise<{ [key: string]: string | string[] | undefined }>` for `searchParams` (or the project's chosen shape), and `await` them before use.
5. For `children` props, prefer `React.ReactNode`.

### 7. Fix configuration

1. Validate `next.config.*` against the installed Next.js major version. Remove keys that are no longer recognized.
2. For image domains, use `images.remotePatterns` (modern) instead of `images.domains` where possible.
3. For static export, set `output: 'export'` and ensure no server-only routes remain.
4. For `trailingSlash`, `rewrites`, `redirects`, ensure they do not conflict with physical routes.
5. Keep the config minimal. Do not add speculative options.

### 8. Fix routing

1. App Router: every route needs a `page.tsx`/`page.jsx` at its segment. `layout.tsx` does not create a route.
2. Pages Router: route files export a default component. `_app` and `_document` are special.
3. Remove duplicate routes that shadow each other (e.g., `app/blog/[slug]/page.tsx` and `pages/blog/[slug].tsx`).
4. Fix catch-all vs optional catch-all placement (`[...slug]` vs `[[...slug]]`).
5. Ensure `not-found.tsx` / `error.tsx` are in valid App Router locations.

### 9. Fix data fetching and server/client boundaries

1. App Router: default components are server components. Add `'use client'` only when using browser APIs or React hooks.
2. Do not use `getServerSideProps`, `getStaticProps`, or `getInitialProps` in App Router components. Move that logic into the server component body or a Server Action/Route Handler.
3. Pages Router: keep `getServerSideProps`/`getStaticProps` when present; fix their return types if broken.
4. Fetching in server components: use the `fetch` API or ORM/direct DB calls. Respect caching semantics; add `{ cache: 'no-store' }` or `revalidate` only when the user's requirements demand it.
5. Server Actions: ensure they are async functions and imported correctly. Do not call a Server Action from a non-async context.

### 10. Fix runtime / SSR issues

1. `window`, `document`, `localStorage`, and `navigator` require a client component with `'use client'` or use inside `useEffect`.
2. Hooks (`useState`, `useEffect`, etc.) require `'use client'` in App Router.
3. Event handlers (`onClick`, `onSubmit`) on custom components require `'use client'` if the component is a server component.
4. Do not mark a component `'use client'` just because it receives a React node; pass nodes as props from server components instead.

### 11. Fix images, fonts, and metadata

1. `next/image`: ensure `src` is a valid string, `StaticImageData`, or URL. For remote images, register the hostname in `next.config.js` `images.remotePatterns`.
2. `next/font`: import from `next/font/google` or `next/font/local`. Do not mix with manual `@font-face` unless intentional.
3. Metadata: in App Router, export `metadata` from `layout.tsx` or `page.tsx`. In Pages Router, use `next/head`.

### 12. Fix dependency and peer-dependency issues

1. Check that React and React-DOM versions match the Next.js peer-dependency range.
2. If Next.js 15+ is installed, React 19 is expected. If Next.js 14, React 18.
3. Run the install command after changing `package.json`. Prefer the lockfile's package manager.
4. Do not add dependencies unless they are required by the fix.

### 13. Re-run diagnostics

After fixes, run the full diagnostic chain again:

1. `lint`
2. `type-check` / `tsc --noEmit`
3. `build`
4. `test` (if present)

Repeat the classify-fix-validate loop until the project is green or only intentional, documented failures remain.

### 14. Report

Give the user a concise summary:

- What was broken (root causes, not every line changed).
- What was fixed.
- What remains red, if anything, and why.
- Any follow-ups (e.g., deprecated API that needs a product decision, missing env vars, tests to add).

## Examples

### Example 1 — Fixing broken imports after a folder move

A file was moved from `app/components/Button.tsx` to `app/_components/Button.tsx`. Imports that used `../components/Button` now fail.

1. Find all broken imports with the diagnostic output.
2. Update them to `../_components/Button` or the alias `@/_components/Button`.
3. Re-run `tsc --noEmit` to confirm.

### Example 2 — Missing `await` on async page params (Next.js 15)

```tsx
// Before — broken
export default function Page({ params }: { params: { id: string } }) {
  return <div>{params.id}</div>;
}

// After — fixed
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  return <div>{id}</div>;
}
```

### Example 3 — Client-only API in a server component

```tsx
// Before — broken
export default function Banner() {
  const width = window.innerWidth; // window is not defined during SSR
  return <div style={{ width }}>Banner</div>;
}

// After — fixed
'use client';

import { useEffect, useState } from 'react';

export default function Banner() {
  const [width, setWidth] = useState(0);
  useEffect(() => {
    setWidth(window.innerWidth);
  }, []);
  return <div style={{ width }}>Banner</div>;
}
```

## Checklist

- [ ] Project router detected (App Router / Pages Router / both).
- [ ] Diagnostics run in order: lint → type-check → build → test.
- [ ] Every failure classified into a bucket before fixing.
- [ ] Imports resolved, paths corrected, dead imports removed.
- [ ] Type errors fixed without widening to `any`.
- [ ] `next.config.*` validated against installed Next.js version.
- [ ] Routing conflicts resolved.
- [ ] Server/client boundaries correct (`'use client'` only where needed).
- [ ] Data fetching uses the router-appropriate pattern.
- [ ] Images/fonts/metadata use current Next.js APIs.
- [ ] Dependencies match Next.js peer requirements.
- [ ] Diagnostics re-run and green (or documented exceptions remain).
- [ ] User informed of root causes, fixes, and any follow-ups.

## References

- [Next.js documentation](https://nextjs.org/docs)
- [App Router](https://nextjs.org/docs/app)
- [Pages Router](https://nextjs.org/docs/pages)
- [next.config.js options](https://nextjs.org/docs/app/api-reference/next-config-js)
- [next/image](https://nextjs.org/docs/app/api-reference/components/image)
- [next/font](https://nextjs.org/docs/app/api-reference/components/font)
- [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)

## Reusable prompts

Optional prompts for this skill live in `prompts/` if the agent needs a focused one-shot prompt for a single Next.js fix.
