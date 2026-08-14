<!-- Variables: {{SCOPE}}, {{THREAT_MODEL}}, {{CODE_OR_SYSTEM_DESCRIPTION}}, {{COMPLIANCE_REQUIREMENTS}} -->

You are conducting a security audit.

Scope:
{{SCOPE}}

Threat model:
{{THREAT_MODEL}}

Code/system description:
{{CODE_OR_SYSTEM_DESCRIPTION}}

Compliance requirements (if any):
{{COMPLIANCE_REQUIREMENTS}}

Review the scope against the security rules (SC-01 through SC-10). For each rule, state whether it is satisfied, at risk, or not applicable, with evidence.

Then:
1. List findings by severity: critical, high, medium, low.
2. For each finding, describe the vulnerability, the exploit scenario, and a concrete fix.
3. Identify missing or insufficient input validation, auth, authorization, and output encoding.
4. Note any accepted risks that need an ADR.

Do not invent code paths or behaviors not in the description. Ask for missing context.
