# Documentation — `DOC`

Rules for documentation that stays useful. Cited as `DOC-NN`.

---

### DOC-01 — Docs are a deliverable, not an afterthought
A change that alters behavior, contracts, setup, or operations is not complete until the affected documentation is updated in the same change. Code without current docs is unfinished work.

### DOC-02 — Document decisions, not just mechanics
Anyone can read the code to learn *what* it does. Document *why*: the constraint, the trade-off, the rejected alternative. Decisions of consequence go in an ADR (`../adr/README.md`); rationale for a localized choice goes in a comment (CQ-09).

### DOC-03 — Single source of truth
Every fact lives in exactly one place; everywhere else links to it. Never copy a procedure, schema, or config into a second document — it will drift. This mirrors the cross-referencing convention in `../system/conventions.md`.

### DOC-04 — Write for the reader's task
Organize docs around what the reader is trying to do (run it, change it, debug it, integrate with it), not around how the code is structured. Lead with the most common path; push edge cases down.

### DOC-05 — Docs decay — date them and own them
A document that describes a moving system must carry an `updated` date and a clear scope. When you find a doc that no longer matches reality, fix it or mark it `stale` (see conventions §5). A wrong doc is worse than no doc.

### DOC-06 — Examples over adjectives
Show a concrete, runnable example instead of describing behavior abstractly. One real request/response, one real command, one real config beats three paragraphs of prose.

### DOC-07 — No placeholder or filler documentation
Do not generate docs to fill a section. No "This function does things" docstrings, no empty `## Usage` headings, no restating the README in the CONTRIBUTING. If a section has nothing to say yet, omit it. (Pairs with CQ-08.)

### DOC-08 — Keep it close to the code
Documentation lives as near as practical to what it describes: README with the module, API docs with the endpoint, architecture notes in `../architecture/`, decisions in `../adr/`. Proximity is what makes DOC-01 achievable.

### DOC-09 — Operational docs are mandatory for anything that runs
If a system is deployed, monitored, or paged for, it needs runbooks: how to deploy, how to roll back, how to read its alerts, how to recover. Code that runs without operational docs transfers risk to whoever is on call.

---

*Format and citation: [README.md](README.md). Conventions: `../system/conventions.md`.*
