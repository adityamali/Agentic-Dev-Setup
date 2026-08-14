---
name: <agent-name>
version: 0.1.0
author: <name or tool>
compatible_agents: []
extends: <base-agent-name>        # optional
---

# <Agent display name>

## Purpose

<!-- What this agent is for. One paragraph. -->

## Not for

<!-- What this agent explicitly does not do. -->

## Capabilities

<!-- What the agent can do. Each capability is a concise statement. -->
-

## Skills loaded

<!-- Skills from ../skills/ this agent uses. -->
- `../skills/<skill-name>/`

## Prompts used

<!-- Prompts from ../prompts/ this agent uses by default. -->
- `../prompts/<prompt-name>.md`

## Instructions

<!-- Specific system instructions for this agent. Keep tight; link to AGENTS.md for global standards. -->

1. Follow `../AGENTS.md` for all global engineering standards.
2. (Add agent-specific instructions here.)

## Specialization

<!-- If this agent extends another, describe what it adds, overrides, or removes. -->

## Composition

<!-- How this agent combines with other agents or skills. -->

## Related

<!-- Links to other agent definitions or ADRs. -->
