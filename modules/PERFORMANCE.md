# Performance Module

Load for explicit performance/scale work or when a changed user workflow is latency-sensitive due to data loading, search, rendering, network calls, or external providers.

## Principle
Measure demonstrated bottlenecks and realistic end-to-end workflows. Do not optimize speculative micro-costs at the expense of correctness, auditability, recoverability, or maintainability.

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
