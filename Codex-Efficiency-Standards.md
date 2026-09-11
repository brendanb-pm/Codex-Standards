# Codex Efficiency Standards — Conditional Module

Load this file only for explicit efficiency tuning, token/compute audits, unusually large tasks, or when repeated execution shows material waste. The normal execution baseline already lives in `Codex-Standards.md` and must not be duplicated here.

## 1. Objective

Eliminate operations with low incremental value while preserving correctness, security, acceptance criteria, and required verification.

Optimize for:
- useful information per read;
- useful implementation progress per edit;
- useful confidence per test;
- minimal redundant context processing;
- minimal repeated tool execution.

## 2. Context Budget

Inspect instruction/context sources before adding more permanent rules.

Load only applicable standards, modules, and documents. Start with targeted files before broad repository exploration; avoid carrying irrelevant conversation history; and prefer structured summaries/checkpoints over raw historical transcripts.

Flag for review:
- any always-loaded instruction file above ~5k tokens;
- combined always-loaded instruction context above ~10k tokens;
- duplicated rules across core/project/agent-specific files;
- broad tool schemas loaded when only a small tool subset is needed.

Prefer a small always-loaded core plus conditional modules. Do not solve token waste by creating dozens of tiny modules that increase discovery/tool overhead.

## 3. Progressive Inspection

Expand only as needed:
1. relevant symbol/file;
2. direct dependency/caller;
3. related tests;
4. adjacent subsystem;
5. broader architecture only when required.

Use exact symbols, work-item IDs, endpoint names, component names, config keys, tests, and error strings before broad repository scans.

## 4. Cache and Context Reuse

Within one task, do not reread unchanged standards, repository maps, architecture, test commands, or files without cause. Repeat only when state changed, evidence was incomplete, new evidence creates doubt, or verification requires it.

When runtime telemetry is available, inspect cache-read, cache-create, uncached input, and output/reasoning cost. Prefer stable reusable prefixes and avoid unnecessary prompt churn that destroys cache reuse.

Never invent cache statistics when the runtime does not expose them.

## 5. Tool Surface

Expose the smallest practical tool set for the task. Defer browser, deployment, issue-tracker, messaging, database, or other large tool schemas until needed.

Audit MCP/tool configuration when:
- many servers are connected by default;
- a proxy/gateway exposes large tool catalogs;
- tool-choice errors or long deliberation recur.

A proxy that hides a large catalog behind deferred discovery is generally preferable to eagerly injecting every schema.

## 6. Subagents

Use subagents only when parallelism or specialization provides clear value. Do not spawn agents for work a single run can complete efficiently.

Subagent model selection follows the deterministic model/priority policy in core. Do not pin a stronger model when inheritance or a lighter reliable model is sufficient.

## 7. Hooks / Output Compression

Use deterministic hooks for cheap repetitive enforcement when they reduce repeated model work: formatting checks, generated-file rejection, test selection, command-output truncation, or noisy-tool summarization.

Do not add hooks whose own latency/complexity exceeds the saved compute. Keep full raw logs available when needed for diagnosis even if routine agent-visible output is compressed.

Tool-output compression may summarize repetitive success, no-op, or noise output only when it preserves errors, warnings, failures, changed state, security-relevant output, test summaries, verification evidence, and unexpected behavior. Never compress evidence needed to diagnose a failure or substantiate completion.

## 8. Scheduled / Background Work

Audit cron jobs, scheduled agents, watchers, and background tasks for intervals shorter than the underlying information's useful lifetime. Avoid repeated work that invalidates cache or fetches unchanged state.

## 9. Verification Cost

Use the core verification ladder. Full suites, builds, installations, network calls, deployment simulations, and large dataset processing require a concrete reason. Ask what new confidence the expensive operation adds and whether a cheaper targeted check can answer first.

## 10. Measurement

Compare representative workloads before and after an optimization. When exposed, track:
- input/context tokens;
- cache reads/creation;
- output/reasoning tokens;
- tool calls;
- elapsed time;
- failed/repeated commands;
- acceptance-pass rate and rework.

Do not optimize a metric in isolation. A token reduction that materially increases defects or rework is not an efficiency improvement.

## 11. Audit Output

For an efficiency audit, report only actionable findings using:

```text
FINDING | SEVERITY | EVIDENCE | COST / IMPACT | RECOMMENDATION
```

Sort by expected cost/benefit. Prefer removing permanent complexity over adding more permanent efficiency rules.
