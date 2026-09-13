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

## 12. Context Efficiency and Retrieval

Correctness, auditability, security, traceability, and required verification take precedence over token or context savings.

1. **Query before traversal.** Before a broad repository read or search, use an appropriate structural graph, index, symbol map, or query when one exists and is fresh enough for the question.
2. **Progressive disclosure.** Prefer, where appropriate: repository graph/query; search result; symbol/signature/map; diff; targeted line range; then complete file. Full-file reads remain required whenever smaller evidence cannot establish correctness.
3. **Repeated-read control.** Do not reread an unchanged artifact solely because it was accessed in an earlier step. Reuse cached or indexed knowledge only when content identity, source freshness, and provenance remain available.
4. **Compute, do not context-dump.** For large logs, datasets, test output, search results, JSON, or generated files, run deterministic extraction that returns the needed result instead of inserting raw source into model context.
5. **Command-output hygiene.** Prefer scoped tests, filtered search, `git diff --stat` before large diffs, machine summaries, and failure-focused output when complete success logs add no evidence. Never hide errors, warnings, changed state, security-relevant output, test summaries, or evidence needed to diagnose a failure.
6. **Preserve authoritative input.** Never semantically compress away user requirements, acceptance criteria, security, business, migration, architecture, compliance rules, or exact diagnostic error evidence.
7. **Reversibility.** A summary or compressed reference must retain a route to its authoritative source and identity where technically feasible.
8. **Dependency minimization.** Prefer repository, language, runtime, or OS-native capabilities when they satisfy the requirement cleanly.

Any optimization that reduces context/token use while reducing correctness, verification strength, or traceability is a regression.

## 13. Repository Intelligence and Graphify Pilot

An AST-derived repository graph is an optional project-scoped intelligence layer, not a substitute for current source. Graphify is the preferred first candidate because its code graph is deterministic and queryable; its results are advisory until direct source inspection confirms material conclusions.

Before installing or executing a third-party graph tool, inspect the pinned release's installation method, lifecycle hooks, network behavior, permission changes, files it writes, code/data locality, and uninstall/recovery path. Do not bypass host trust prompts, install globally merely for a pilot, or add hooks without explicit review.

When approved for a project pilot:

1. use the tool's project-scoped Codex integration rather than machine-wide configuration where supported;
2. record tool version, repository commit, graph generation time, source scope, and whether any non-code semantic/network pass is enabled;
3. ignore generated graph/index output by default unless the project explicitly needs a reviewed artifact committed;
4. refresh the graph after pull, merge, rebase, or relevant source change before relying on it;
5. query the graph before broad traversal, then inspect direct source whenever results are missing, stale, ambiguous, inferred, or material to correctness;
6. never let stale graph data override the current worktree or authoritative source.

Bootstrap, refresh, and removal commands must be documented in the adopting project's instructions. No graph tool is mandatory until a controlled cohort demonstrates equal-or-better correctness and verification.

For Graphify's documented Codex path, the adopting project records the reviewed, pinned equivalent of `graphify install --project --platform codex` for bootstrap, `graphify .` for build/refresh, and `graphify query "<question>"` for retrieval. Removal must first preview the project files written by the chosen version, then remove only those reviewed project-scoped integration and generated-output paths; do not use global uninstall or hook commands for a project pilot.

## 14. Optional Compression / Caching Pilot

LeanCTX is the preferred first compression/caching experiment candidate. It is not mandatory and must be evaluated project-by-project after the Graph cohort. The first comparison must not simultaneously deploy Headroom, Context Mode, Token Optimizer MCP, or another general-purpose context-compression layer.

Before a LeanCTX pilot, inspect its installer, hooks, MCP registration, proxy behavior, telemetry/update settings, network/data egress, files changed, permissions, and dry-run uninstall/recovery path. Start with the smallest project-scoped, read-path configuration; keep request-proxy/wire compression, cloud sync, automatic hooks, and machine-wide settings disabled unless separately reviewed and explicitly authorized.

Evaluate cached rereads, scoped reads, command compression, reversibility, security/data locality, existing `AGENTS.md` behavior, provider-reported usage when exposed, and correctness under unchanged acceptance and verification gates. A failed safety or compatibility review is `BLOCKED`, not a reason to weaken controls.

## 15. Context-Efficiency Measurement and Experiments

`telemetry/context-efficiency-measurement.schema.json` defines optional enrichment records for the AI-delivery ledger. Its `schema_version` is independent of the EFF delivery-record `v` field. It complements EFF v2; do not add fields to EFF v2 or fabricate unavailable values.

Capture when observable: story, project, agent/client, requested/runtime model and reasoning configuration, cohort, files inspected, repeated file reads, broad searches, graph/index queries, command/tool calls, raw bytes produced, bytes returned to the model, locally estimated input tokens, cached input tokens, output tokens, expansion/retrieval events, elapsed time, test/acceptance/correctness results, human rework, commit SHA, and standards SHA.

When directly and reliably observable, execution timestamps, deterministically derived elapsed time, and requested/runtime model or priority configuration MUST be recorded. Use null only when the source does not expose the value reliably; never infer, substitute, or backfill it.

Keep these measures distinct: observed bytes/context avoided; locally estimated token reduction; provider-reported token usage; and estimated monetary savings. Never present local estimates as provider-billed usage or savings, and use `null` rather than inventing evidence.

Use controlled cohorts with identical acceptance criteria and verification gates:

- `BASELINE` — current behavior without an additional Graphify/LeanCTX layer;
- `GRAPH` — this standard plus Graphify;
- `GRAPH_CONTEXT_OPTIMIZATION` — this standard plus Graphify and LeanCTX.

Compare efficiency and correctness. A cohort is not superior merely because it uses fewer tokens, bytes, reads, or calls; it must retain acceptance, verification, and supported later-outcome quality.
