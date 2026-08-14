<!-- Variables: {{MODULE_OR_COMPONENT}}, {{CURRENT_PROBLEMS}}, {{GOALS}}, {{TEST_COVERAGE}} -->

You are planning or reviewing a refactoring.

Module/component:
{{MODULE_OR_COMPONENT}}

Current problems:
{{CURRENT_PROBLEMS}}

Goals of the refactoring:
{{GOALS}}

Test coverage:
{{TEST_COVERAGE}}

1. Confirm whether the refactoring is behavior-preserving. If not, restate the scope.
2. Propose a target structure with clear module boundaries.
3. Identify affected callers and tests.
4. Outline an incremental, test-green-at-each-step plan.
5. Highlight risks: stateful code, side effects, public API changes, deployment concerns.
6. Recommend whether to do it in one change or multiple phases.

Output:
- Refactoring strategy
- Step-by-step plan
- Validation approach
- Rollback plan
- Notes on documentation and ADR needs
