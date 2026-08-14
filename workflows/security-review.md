---
id: WF-07
title: Security review
---

# WF-07: Security review

## Trigger

Reviewing code, architecture, or infrastructure for security issues. Use this before shipping sensitive features, handling untrusted input, or integrating with external identity/data systems.

## Phase 1 — Scope

- [ ] Identify what is being reviewed: code, API, dependency, deployment, or process.
- [ ] Identify the threat model: who can attack what, and what is the impact.
- [ ] Determine if sensitive data, auth, or authorization is involved.

## Phase 2 — Checklist review

- [ ] Input validation at every boundary (SC-02).
- [ ] Authentication on every endpoint/action (SC-10).
- [ ] Authorization checks against the specific resource (SC-10).
- [ ] No secrets in code or logs (SC-05).
- [ ] Output encoded for its context (SC-06).
- [ ] Failures deny by default (SC-07).
- [ ] Dependencies are pinned and justified (SC-09).
- [ ] Encryption in transit and at rest where required (SC-08).

## Phase 3 — Deep inspection

- [ ] Review auth/session/token handling.
- [ ] Review data access patterns and least-privilege roles (SC-04).
- [ ] Check for injection risks: SQL, command, LDAP, HTML/JS (SC-03, SC-06).
- [ ] Review error messages for information leakage.
- [ ] Review logging for sensitive data.

## Phase 4 — Remediate

- [ ] Prioritize findings by likelihood and impact.
- [ ] Fix the highest-priority issues before shipping.
- [ ] For accepted risks, document the rationale (ADR if significant).
- [ ] Add regression tests for any fixed vulnerability.

## Phase 5 — Document

- [ ] Summarize findings and remediation for the user.
- [ ] Update `architecture/<slug>/` security notes if relevant.
- [ ] Create an ADR for accepted risks or consequential security decisions.
- [ ] Create/update memory notes for security patterns or gotchas.

## Exit criteria

- [ ] All high-impact findings remediated or explicitly accepted.
- [ ] Regression tests added for fixed issues.
- [ ] Findings and decisions are documented.
