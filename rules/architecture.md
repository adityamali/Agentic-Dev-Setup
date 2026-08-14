# Architecture — `AR`

Rules for system structure. Cited as `AR-NN`. These govern *between-unit* decisions; `code-quality.md` governs within-unit decisions. Significant architectural choices are recorded in an ADR (see `../adr/README.md`).

---

### AR-01 — Dependencies point inward
High-level policy must not depend on low-level detail. Domain logic depends on abstractions; infrastructure (databases, HTTP, frameworks, filesystem) implements those abstractions. The dependency arrow points toward stability. Never let a core module import a framework or a driver directly.

### AR-02 — Separate concerns by change rate
Group code that changes for the same reason and at the same rate; separate code that changes for different reasons. Business rules, persistence, transport, and presentation are different concerns — keep them in different modules with explicit boundaries.

### AR-03 — Boundaries are explicit and narrow
Every interaction between modules/services goes through a defined interface. No reaching into another module's internals, no shared mutable state, no "temporary" cross-boundary shortcuts. The cost of a boundary is paid up front; the cost of a missing one compounds.

### AR-04 — Design for today's scale, enable tomorrow's
Build for the load and complexity you actually have, structured so the obvious next step is cheap. Do not build distributed-systems machinery for a single-process problem, and do not paint yourself into a corner that makes the known next requirement impossible. Scalability is a direction, not a starting point.

### AR-05 — Extensibility via seams, not switches
Make the system open to extension at well-defined seams (interfaces, plugins, events) rather than by editing stable code or threading boolean flags through call stacks. Add a seam only where variation is real or imminent — otherwise it's CQ-06 speculative abstraction.

### AR-06 — Shared modules are a liability to manage
Code shared across services/modules creates coupling. Share only stable, well-versioned contracts and genuinely common utilities. Prefer duplication of unstable logic over premature sharing (pairs with CQ-04). A shared module is a public API — treat its changes accordingly.

### AR-07 — Data flows in one understandable direction
Within a boundary, make data flow traceable: who produces it, who transforms it, who consumes it. Avoid circular flows, hidden side-channels, and mutation-at-a-distance. If you can't draw the flow, neither can the next engineer — or the next incident responder.

### AR-08 — Failures are part of the design
Every integration point has a defined failure behavior: timeout, retry, backoff, circuit-break, degrade, or propagate. "It shouldn't fail" is not a strategy. Decide at design time and record the decision — see `security.md` for failure modes that become vulnerabilities.

### AR-09 — Operational simplicity is a feature
Fewer moving parts beats more. Each service, queue, cache, and datastore must justify its operational cost (deploy, monitor, secure, debug, recover). Reject a component whose benefit doesn't clearly exceed its lifetime operational burden.

### AR-10 — Record consequential decisions
Any decision that is expensive to reverse, affects multiple components, or chooses among real alternatives gets an ADR. Memory notes capture knowledge; ADRs capture *decisions*. See `../adr/README.md` for the mandatory triggers.

---

*Format and citation: [README.md](README.md). Conventions: `../system/conventions.md`.*
