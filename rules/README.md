# Rules

Citable engineering rules. These are the enforceable standards behind the principles in `../AGENTS.md`. AGENTS.md states *what* we value; these files state *the rules* precisely enough to apply and to cite.

## How to use

- Each rule has a stable ID (`CQ-03`, `AR-02`, …) defined in `../system/conventions.md`.
- **Cite, don't restate.** In a plan, review, or ADR, write `per SC-01` rather than re-explaining the rule.
- Rules are normative. If you deviate, say so explicitly and justify it — ideally in an ADR.
- Rules are technology-agnostic. Technology-specific guidance belongs in a skill (`../skills/`) or a memory note, not here.

## The rule books

| File | Prefix | Domain |
|------|--------|--------|
| [code-quality.md](code-quality.md) | `CQ` | Readability, simplicity, modularity, duplication, anti-slop, anti-overengineering |
| [architecture.md](architecture.md) | `AR` | Dependency direction, separation of concerns, scalability, extensibility |
| [testing.md](testing.md) | `TS` | Testing philosophy, validation, regression prevention |
| [security.md](security.md) | `SC` | Secure defaults, input validation, least privilege, secrets |
| [documentation.md](documentation.md) | `DOC` | Documentation standards and update expectations |

## Changing a rule

Rules change rarely and deliberately. To add, amend, or retire one, follow the trigger in `../system/maintenance.md` and, when the change is a real decision with trade-offs, record it in an ADR. Never edit a rule's meaning silently — rules are cited by ID, so a silent change corrupts every citation.
