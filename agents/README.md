# Agents

Framework for defining and discovering agents. This directory does not contain dozens of specialized agents; it defines the standard for how agents are described, discovered, inherited, specialized, and composed.

## What is an agent definition?

An agent definition is a markdown file that describes:

- **Metadata** — name, version, author, compatible tools.
- **Purpose** — what the agent is for and what it is not for.
- **Capabilities** — what it can do, including skills it loads.
- **Specialization** — which base agent it extends, if any.
- **Composition** — how it combines with other agents or skills.
- **Prompts / instructions** — specific system instructions.

Agents are **personas**; skills are **capabilities**. An agent definition loads skills, not the other way around.

## File layout

```text
agents/
├── README.md
└── <agent-name>/
    └── agent.md
```

Use the template in [`_template/agent.md`](_template/agent.md) when creating a new agent.

## Discovery

Agents are discovered by reading this directory. A tool or orchestrator loads the `agent.md` for the agent it wants to run.

## Inheritance

An agent may declare a `extends` field pointing to another agent in this directory. The extending agent inherits capabilities and may:

- Add new capabilities.
- Override instructions (state overrides explicitly).
- Remove a capability only if it documents why.

## Specialization

Specialization = narrowing an agent's purpose. A specialized agent inherits from a general one and adds domain focus. Example: `general-engineer` → `frontend-engineer` → `react-engineer`.

## Composition

An agent may compose with skills from `../skills/` and with prompts from `../prompts/`. Composition is declared in the agent definition, not implicit.

## Creating a new agent

1. Copy [`_template/agent.md`](_template/agent.md).
2. Fill metadata, purpose, capabilities, and instructions.
3. Add it to this README under the registry.
4. If it changes how agents are defined or discovered, record the decision in an ADR.

## Registry

<!-- Add agents here as they are created. -->

| Agent | Extends | Purpose |
|-------|---------|---------|
|       |         |         |
