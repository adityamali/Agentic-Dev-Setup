# Code Quality — `CQ`

Rules for readable, maintainable, minimal code. Cited as `CQ-NN`.

---

### CQ-01 — Optimize for the reader
Code is read far more than it is written. Choose clarity over cleverness. A name, structure, or control flow is good when a competent engineer unfamiliar with the change understands it on first read.

### CQ-02 — Names carry meaning
Names reveal intent, units, and scope. No `data`, `tmp`, `res`, `x` outside trivial loops. Booleans read as predicates (`isReady`, `hasAccess`). Functions read as verbs; types as nouns. A name that needs a comment to be understood is the wrong name.

### CQ-03 — One reason to change
Each function, module, and class has a single, statable responsibility. If you need "and" to describe what a unit does, split it. Length is a symptom, not the rule — a cohesive 80-line function beats a fragmented 20-line one.

### CQ-04 — Don't Repeat Yourself — but don't guess
Eliminate *duplicated logic* (the same rule expressed twice), not *similar-looking code* (two things that happen to look alike today). Two copies are cheaper than a wrong abstraction. Extract only when the duplication is real and stable — usually on the third occurrence, not the first.

### CQ-05 — Simplest thing that works
Prefer the simplest solution that fully satisfies the requirement. No configurability nobody asked for, no generics "for later", no plugin systems for one plugin. If a simpler design passes the same tests, the simpler design wins.

### CQ-06 — No speculative abstraction
Do not build for hypothetical futures. An abstraction must pay for itself *now* by removing real duplication or hiding real complexity. See `architecture.md` AR-04 for the architectural side of this rule.

### CQ-07 — No dead code
Delete unused functions, parameters, branches, files, and commented-out code. Version control is the archive. Dead code is a liability, not a keepsake.

### CQ-08 — No AI slop
Do not emit generic boilerplate: restating-the-obvious comments, docstrings that paraphrase the signature, `// TODO: implement` left behind, defensive checks for impossible states, or "flexible" wrappers around a single call. Every line must earn its place. If a comment says what the code says, delete the comment; if it says *why*, keep it.

### CQ-09 — Comments explain why, code explains what
Comment intent, invariants, and non-obvious constraints. Never narrate the mechanics. Keep comments adjacent to the code they describe and update them when the code changes — a stale comment is worse than none.

### CQ-10 — Handle errors deliberately
Every failure mode is either handled, propagated with context, or explicitly allowed to crash — never silently swallowed. No empty `catch`, no `try/except: pass`, no returning `null` to mean five different things. Fail fast and loudly at the boundary; be specific about what went wrong.

### CQ-11 — Consistency within a codebase
Match the existing style, patterns, naming, and structure of the project you're editing, even when you'd choose differently. Introduce a new pattern only deliberately and document it (ADR if significant). Two idioms for one thing is a defect.

### CQ-12 — Minimal diff
Make the smallest change that achieves the goal. Do not reformat, reorder, or "improve" unrelated code in the same change. Reviewers and `git blame` must be able to see exactly what the change was.

---

*Format and citation: [README.md](README.md). Conventions: `../system/conventions.md`.*
