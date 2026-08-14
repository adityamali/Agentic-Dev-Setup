# Testing — `TS`

Rules for validating software and preventing regressions. Cited as `TS-NN`.

---

### TS-01 — Test behavior, not implementation
Tests assert observable behavior and contracts, not internal mechanics. A test that breaks when you refactor without changing behavior is a bad test. Couple tests to the public surface, not to private structure.

### TS-02 — The pyramid, honestly
Many fast unit tests, fewer integration tests, very few end-to-end tests. Push confidence down to the cheapest level that can provide it. An E2E suite that duplicates what unit tests already prove is cost without signal.

### TS-03 — Every bug gets a regression test
A fix is not complete until a failing-before / passing-after test reproduces the original bug. This is how the same bug never ships twice. Pairs with workflow `WF-02` (fix-bug).

### TS-04 — Validate at the boundary
Validate and test inputs where they enter the system — request payloads, file uploads, env vars, external responses. Internal code may trust already-validated data; the boundary may not. Pairs with `security.md` SC-02.

### TS-05 — Tests are first-class code
Tests follow the same quality rules as production code (CQ-01…CQ-12): clear names, no duplication of setup that obscures intent, no dead tests, no commented-out asserts. A test you can't read can't be trusted.

### TS-06 — No flaky tests, no skipped tests
A flaky or permanently-skipped test erodes the whole suite's credibility. Fix it or delete it. Never silence a failure by skipping. Quarantine is a temporary state with an owner and a deadline, not a strategy.

### TS-07 — Determinism or explicit isolation
Tests must not depend on execution order, wall-clock time, network, or shared mutable state unless that dependency is the thing under test and is explicitly isolated/mocked. Parallelizable by default.

### TS-08 — Name tests as specifications
A test name states the scenario and the expected outcome: `rejects_charge_when_card_expired`, not `testCharge`. Reading the test names should describe the feature's contract.

### TS-09 — Coverage is a floor, not a goal
Chase *confidence in the risky paths*, not a percentage. Critical logic, error handling, and boundary conditions get tested first. 100% line coverage with no assertion on behavior is worthless; 70% with the risky paths covered can be enough.

### TS-10 — Prove it runs
A test that has never been run is a rumor. After writing or changing tests, run them and confirm they fail for the right reason and pass for the right reason. Never claim "tested" without a green run you actually saw.

---

*Format and citation: [README.md](README.md). Conventions: `../system/conventions.md`.*
