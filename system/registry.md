# Skill Registry

The canonical registry of skills available to agents. It mirrors `.skill-lock.json` for lock-managed skills and lists locally authored skills.

## Lock-managed skills

Installed via skill CLI. Source of truth for versions is `.skill-lock.json`.

| Name | Source | Installed | Updated | Status |
|------|--------|-----------|---------|--------|
| agents-sdk | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| cloudflare | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| cloudflare-email-service | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| cloudflare-one | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| cloudflare-one-migrations | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| durable-objects | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| sandbox-sdk | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| turnstile-spin | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| web-perf | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| workers-best-practices | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |
| wrangler | cloudflare/skills | 2026-07-27 | 2026-07-27 | active |

## Locally authored skills

| Name | Path | Status |
|------|------|--------|
|      |      |        |

## Maintenance

- Update this table when installing/removing skills.
- Do not edit lock-managed skill content.
- Local skills are tracked in git; lock-managed skill content is ignored.

See `../skills/README.md` for install/remove conventions and `../system/tool-wiring.md` for skill mirrors.
