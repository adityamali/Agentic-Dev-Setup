# Maintenance

This repository improves only if agents maintain it. This file defines the triggers and procedures for keeping the knowledge base clean, accurate, and useful.

## 1. Memory maintenance

### Create a memory when

- You learned something non-obvious that the next engineer/agent should know.
- You solved a bug and the symptom/cause/fix pattern is worth preserving.
- A project reveals a constraint, quirk, or stable pattern.
- You validated a technology assumption (gotcha, limitation, version behavior).
- A decision becomes stable knowledge (promote from ADR or research note).

### Update a memory when

- Its facts change.
- A new project applies the concept (add a backlink).
- You discover a better formulation.

### Archive a memory when

- It is factually obsolete and no one references it.
- It has been superseded by another note or an ADR.
- A research note's findings were promoted.

### Merge duplicate knowledge when

- Two notes state the same fact.
- Two notes cover the same concept from slightly different angles.

**Procedure:** consolidate into the older or broader note, redirect the other note to it with a note in its body, update `memories/INDEX.md`, and move the redirected note to `memories/archive/`.

## 2. ADR maintenance

### Create an ADR when

- A decision is expensive to reverse.
- A decision crosses module/service boundaries.
- A decision chooses among real alternatives.
- A decision changes security, privacy, availability, or operational behavior.
- A decision introduces a new dependency, datastore, or deployment target.
- A decision establishes a pattern other projects may copy.

### Update an ADR when

- Its status changes (`proposed → accepted`, `accepted → deprecated/superseded`).
- A related ADR is created.

## 3. Architecture maintenance

### Update architecture docs when

- You add/remove/rename a component or service.
- You change a significant data flow.
- You add or replace a dependency.
- You modify a service boundary.
- You change deployment, infrastructure, or external integrations.

## 4. Skill maintenance

### Recommend creating a new skill when

- The same domain knowledge is needed across multiple projects.
- Existing skills are wrong or incomplete and editing them is not possible.
- A workflow would benefit from a reusable, self-contained capability.

### Update the skill registry when

- Installing or removing any skill.
- A skill's source or status changes.

## 5. Workflow maintenance

### Recommend updating a workflow when

- A step is repeatedly skipped or found useless.
- A new mandatory concern appears (e.g., a new security check).
- A workflow's output shape is inconsistent with the rest of the system.

## 6. Rule maintenance

### Recommend updating a rule when

- A rule is repeatedly violated with good reason.
- A rule conflicts with another rule.
- A new category of quality concern is not covered.

Rule changes are structural. Record them in `changelog.md`; use an ADR when the change is a real decision with trade-offs.

## 7. Periodic review

- Once per month (or after ~10 new memory notes): scan `memories/INDEX.md` for duplicates and stale links.
- Once per quarter: review `adr/INDEX.md` for `proposed` ADRs older than 30 days and `stale` architecture docs.
- After any structural change to `system/`: update `system/changelog.md`.

## 8. Agent close-out check

After a task, ask: did I leave memory, ADRs, or architecture docs more accurate than I found them? If yes, do it now. If no, move on.
