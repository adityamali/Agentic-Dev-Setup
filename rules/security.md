# Security — `SC`

Rules for building software that is secure by default. Cited as `SC-NN`. Security failures are also design failures — see `architecture.md` AR-08.

---

### SC-01 — Secure by default
The default configuration is the secure one. Security must not depend on a user remembering to turn it on. Authentication on, encryption on, minimal permissions, verbose errors off in production — out of the box.

### SC-02 — Never trust input
Treat all external data as hostile: user input, API payloads, uploaded files, env vars, URLs, headers, webhook bodies, and responses from third-party services. Validate type, range, length, and format at the boundary (pairs with `testing.md` TS-04). Reject by default; allow-list rather than block-list.

### SC-03 — Parameterize, never interpolate
Build queries, commands, and shell invocations with parameterized APIs and prepared statements — never string concatenation of untrusted data. This one rule eliminates SQL injection, command injection, and most XSS-by-construction.

### SC-04 — Least privilege everywhere
Every identity, token, service, and process gets the minimum permissions for its actual task, for the minimum time. No wildcard IAM, no shared admin credentials, no long-lived broad-scope tokens "for convenience." Scope down; rotate; expire.

### SC-05 — Secrets never touch code or logs
No secrets in source, config committed to VCS, comments, tests, or logs. Load secrets from a manager or environment at runtime. Redact tokens/keys/PII from logs and error messages. If a secret ever appears in a diff, rotate it — it is compromised.

### SC-06 — Output is encoded for its context
Data rendered into HTML, SQL, shell, LDAP, URLs, or JSON is encoded/escaped for *that* context at the point of use. Encoding is context-specific; one generic "sanitize" function does not exist.

### SC-07 — Fail closed
On error, deny. Auth/authorization checks, permission evaluations, and safety guards must default to the restrictive outcome when something goes wrong. A parser crash must never become an auth bypass.

### SC-08 — Minimize and protect data
Collect only the data you need, keep it only as long as needed, encrypt it in transit and at rest, and never log PII/credentials. The cheapest data to breach is the data you never stored.

### SC-09 — Dependencies are attack surface
Every dependency is code you ship. Add them deliberately, pin versions, review them, and keep them updated. Remove dependencies you no longer use. A smaller dependency tree is a smaller target.

### SC-10 — Authentication and authorization are not optional add-ons
Design them in from the start. Authenticate every request, authorize every action against the specific resource, and never rely on obscurity (hidden URLs, "internal" endpoints) as a control. Server-side checks only — client checks are UX, not security.

---

*Format and citation: [README.md](README.md). Conventions: `../system/conventions.md`.*
