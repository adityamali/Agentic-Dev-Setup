# Skills

The skills directory is a package repository. It currently contains lock-managed skills installed by a skill CLI (tracked in `.skill-lock.json`) and will also hold locally authored skills.

## Layout

```text
skills/
├── README.md              # this file
├── SPEC.md                # standard skill format
├── _template/             # annotated template for new skills
├── .skill-lock.json       # lock file for vendored skills
└── <skill-name>/          # one directory per installed/ authored skill
    ├── SKILL.md
    └── (optional resources)
```

## Two kinds of skills

| Kind | Source | Editable | In git? |
|------|--------|----------|---------|
| Lock-managed | Installed via skill CLI (e.g., `npx skills add`) | No | Content ignored; lock file tracked |
| Locally authored | Created by you/agent in this repo | Yes | Tracked |

Do not edit lock-managed skills. If one is wrong, update or replace the package at its source, or create a local override skill.

## Installing a vendored skill

Use your skill CLI. After installation:

1. Confirm `.skill-lock.json` was updated.
2. Add the skill folder to `.gitignore` (lock-managed content is not committed).
3. Add the skill to `../system/registry.md`.

## Installing a locally authored skill

1. Copy `skills/_template/` to `skills/<skill-name>/`.
2. Rename and fill in `SKILL.md` per `SPEC.md`.
3. Do **not** add the folder to `.gitignore`.
4. Register it in `../system/registry.md`.

## Removing a skill

1. Remove it via the skill CLI (lock-managed) or delete the folder (local).
2. Update `.gitignore` if it was listed there.
3. Remove it from `../system/registry.md`.
4. Record the removal in `../system/changelog.md` if other agents depend on it.

## Dependencies

A skill declares dependencies in its `SKILL.md` frontmatter. Dependency resolution is manual: the agent loading the skill must ensure dependencies are installed and loaded. There is no package manager here other than the skill CLI for vendored skills.

## Indexing

The canonical index is `../system/registry.md`. Tools may scan `skills/*/` for `SKILL.md` files, but must ignore `skills/_template/` and anything listed in `.gitignore`.

## Standard format

See [`SPEC.md`](SPEC.md) and [`_template/SKILL.md`](_template/SKILL.md).

## Skill mirrors

Some tools keep their own skill directories (e.g., `~/.claude/skills/`). This repo is the source of truth. Mirrors are managed by your sync tooling, not by editing skills in both places. Document mirror state in `../system/tool-wiring.md`.
