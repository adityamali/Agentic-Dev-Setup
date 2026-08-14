<!-- Variables: {{SYSTEM_OR_ENDPOINT}}, {{METRIC}}, {{TARGET}}, {{BASELINE}}, {{MEASUREMENTS}} -->

You are analyzing a performance problem.

System/endpoint:
{{SYSTEM_OR_ENDPOINT}}

Metric to improve:
{{METRIC}}

Target:
{{TARGET}}

Baseline:
{{BASELINE}}

Available measurements/profile data:
{{MEASUREMENTS}}

1. Identify the most likely bottleneck (CPU, I/O, memory, network, contention).
2. Propose the 2–3 changes with the highest expected impact, ranked.
3. For each change, estimate the expected improvement and the implementation risk.
4. Note any changes that would harm correctness, maintainability, or operational simplicity.
5. Recommend a validation plan: how to measure before and after.

Do not suggest optimizations without a measured bottleneck. Be specific about what to measure next if data is insufficient.
