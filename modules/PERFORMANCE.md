# Performance Module

Load for explicit performance/scale work or when a changed user workflow is latency-sensitive due to data loading, search, rendering, network calls, or external providers.

## Principle
Measure demonstrated bottlenecks and realistic end-to-end workflows. Do not optimize speculative micro-costs at the expense of correctness, auditability, recoverability, or maintainability.

## Evidence-Based Performance

Before modifying code for a performance story:

1. Identify the authoritative performance acceptance criteria.
2. Establish a reproducible baseline for the relevant workload and environment.
3. Measure whether any acceptance threshold actually fails.
4. Identify a verified bottleneck before proposing optimization.

Do not optimize based solely on intuition, generalized best practices, theoretical inefficiency, code appearance, speculative scale concerns, or a requirement that every story produce a diff.

A performance story may successfully conclude with no code changes when the measured baseline satisfies all documented acceptance criteria, no meaningful bottleneck or performance defect is identified, verification confirms the result, and the evidence is retained. In that case, preserve the existing implementation and report:

```text
IMPLEMENTATION CHANGES: NONE
BASELINE: PASS
BOTTLENECK FOUND: NO
VERIFICATION: PASS
EVIDENCE RETAINED: YES
```

This is a complete evidence-backed validation outcome, not `FAILED`, `BLOCKED`, or `INCOMPLETE`. Normal EFF v2 evidence still applies: record `files: 0` when accurate, `verify: "PASS"`, and `compliance: "PASS"`; report the passing baseline and absence of a bottleneck in the narrative or retained measurement artifact without adding EFF fields.

Optimization is justified when measurement shows that documented acceptance criteria are not met, a reproducible bottleneck exists, resource use materially exceeds an approved budget, latency/throughput/scalability fails an applicable requirement, profiling identifies a material hot path, or production-representative evidence demonstrates a performance defect. Implement the smallest evidence-backed change necessary, rerun the same measurement method, and retain both baseline and optimized results for comparison.

Do not retain a speculative performance change merely to demonstrate activity, especially when it has no measured need or benefit, increases complexity or operational risk, or weakens readability or maintainability. Prefer proven adequacy over unnecessary code churn.

Prefer:
- bounded queries and pagination;
- projection of required fields only;
- lazy loading of secondary/non-visible content;
- batching where it reduces round trips safely;
- avoiding repeated full-dataset reads;
- avoiding redundant initialization/rendering/calculation;
- asynchronous provider work when ordinary workflow need not block on it.

## Interactive targets
Unless project-specific targets supersede these diagnostic goals:
- acknowledge normal user input quickly (~100 ms target);
- aim for meaningful content within ~500 ms for common local/fast paths;
- show visible progress when work cannot complete promptly.

End-user latency is authoritative; component budgets may overlap and are diagnostic rather than strict arithmetic sums.

## Verification
Use representative data volumes and realistic workflows. Distinguish backend/component timing from actual operator-perceived performance. Unit tests alone are not performance evidence.

Retain, as applicable, the acceptance thresholds, environment/configuration, workload or dataset size, sample count and method, baseline results, relevant percentiles or aggregates, identified bottleneck, before/after comparison, and measurement limitations. Baseline and post-change methods must remain comparable. Do not claim production performance from measurements that are not production-representative without qualification.
