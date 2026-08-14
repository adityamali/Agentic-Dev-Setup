<!-- Variables: {{SYMPTOM}}, {{REPRODUCTION_STEPS}}, {{ENVIRONMENT}}, {{RECENT_CHANGES}} -->

You are debugging a software defect.

Symptom:
{{SYMPTOM}}

Reproduction steps:
{{REPRODUCTION_STEPS}}

Environment:
{{ENVIRONMENT}}

Recent changes:
{{RECENT_CHANGES}}

Guide me through a structured debugging session. Do not jump to a fix.

1. Formulate 3–5 concrete hypotheses ranked by likelihood.
2. For each hypothesis, propose a fast, safe experiment to confirm or reject it.
3. Identify the logs, metrics, or code paths to inspect.
4. Suggest the narrowest change that would prove the root cause.
5. Once a hypothesis is confirmed, outline the fix and the regression test.

If information is missing, ask for it. Do not invent file names, versions, or error messages.
