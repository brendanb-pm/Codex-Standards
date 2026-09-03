# Codex Standards — Core

## 1. Purpose

This file is the **always-loaded core** for Codex-assisted development. It defines only rules that are broadly applicable to nearly every task. Specialized rules live in conditional modules under `modules/` and must be loaded only when their trigger applies.

Primary objective:

`MIN CONTEXT -> DETERMINISTIC CHECKS -> LOWEST RELIABLE COMPUTE -> MEASURE OUTCOME`

Correctness, security, explicit acceptance criteria, and required verification take precedence over compute savings.

## 2. Instruction Precedence

Apply instructions in this order:

1. Explicit current user instruction.
2. Project/repository instructions, including applicable `AGENTS.md` files.
3. This core standard.
4. Applicable conditional modules selected under Section 5.
5. Default agent behavior.

Project rules may tighten this standard but must not silently weaken safety, verification, or explicit acceptance criteria. Surface material conflicts.

## 3. Codex Execution Brief

Use this compact structure by default:

```text
[WORK-ITEM-ID when applicable]

GOAL
MODEL / PRIORITY
CONTEXT
SCOPE
CONSTRAINTS
ACCEPTANCE
VERIFY
STANDARDS
OUTPUT
```

A numbered story/ticket begins with its exact tracked identifier and no prefix such as `Story:` or `#`.

Keep briefs execution-oriented. Put broad reasoning, tradeoff analysis, and story development outside the Codex brief when possible.

For a materially phased story, identify `PHASE A`, `MIDSTREAM QA/QC GATE`, `PHASE B`, and `FINAL QA/QC GATE` when those labels improve execution clarity. Do not require phase labels for trivial single-phase work.

### Mobile / Desktop prompt mode

- `M:` activates persistent Mobile Mode: compress aggressively while preserving implementation-critical constraints, acceptance, and verification.
- `D:` activates persistent Desktop Mode: include useful detail but still avoid duplicated standards and irrelevant history.
- Do not reproduce canonical standards in prompts when Codex can read them from the repository.

## 4. Deterministic Model / Priority Selection

Select the model and priority **once before execution**. After selection, do not continue discussing, reconsidering, or narrating model choice during the run.

**Execution-profile stability:** Once substantive execution begins, keep the selected model, priority/effort, execution mode, loaded standards modules, and core tool surface stable for that run when practical. Do not toggle configuration merely to seek incremental compute savings. If newly discovered scope requires a materially different execution profile, preserve completed work and restart at a clean boundary rather than repeatedly mutating the active session.

Cache preservation is an efficiency objective, not a correctness requirement. Never avoid necessary context, tools, verification, or escalation solely to preserve a cache hit.

Explicit user selection wins. Otherwise classify the task deterministically:

### T0 — Mechanical
All must be true:
- documentation, comments, formatting, simple configuration, or similarly mechanical work;
- expected change is highly localized (normally <=2 files);
- no meaningful runtime behavior, public contract, security boundary, schema, migration, infrastructure, or production mutation.

Use: **least-cost reliable coding model + Low priority**.

### T1 — Bounded implementation
Default tier when T0, T2, and T3 do not apply. Typical examples: localized bug fixes, normal bounded features, routine tests, or one-subsystem implementation.

Use: **standard reliable coding model + Medium priority**.

### T2 — High-risk but bounded
Use when exactly one high-risk trigger below applies and the requirement/root cause/change surface is otherwise well understood and bounded:
- authentication, authorization, permissions, tenant boundary, or security-sensitive logic;
- schema, migration, data integrity, destructive lifecycle change, or durable recovery;
- infrastructure, deployment, credentials, or production/external-system mutation;
- concurrency, idempotency, uncertain mutation outcome, or broad compatibility contract.

Use: **strong coding/reasoning model + Medium priority**.

### T3 — High-risk and complex
Use when any high-risk trigger applies **and** at least one of these is also true:
- work crosses multiple subsystems or architectural layers materially;
- root cause or requirements are materially ambiguous;
- migration/production action has difficult rollback or large blast radius;
- two or more high-risk triggers apply;
- regression surface is broad or architecture is being changed materially.

Use: **strong coding/reasoning model + High priority**.

ChatGPT maps `least-cost`, `standard`, and `strong` to the current available model family at brief-generation time. Do not hard-bind this standard to permanent model names.

Every brief contains exactly one concise selection line:

```text
MODEL / PRIORITY
<Model> — <Priority>
```

Do not add change-alert headers or compare against the previous brief.

If execution later reveals a higher tier than selected, do not claim a model/priority switch. Continue only if safe under the launched run; otherwise stop at a clean boundary and report once:

```text
RESTART REQUIRED: <tier/model/priority> — <reason>
```

If the task proves simpler, keep the selected model and reduce unnecessary work. Do not discuss downgrading mid-run.

## 5. Conditional Module Loading

Always load only:
- this file; and
- applicable project/descendant `AGENTS.md` instructions.

Select applicable modules **before substantive execution** from the known task scope and load them once. **Do not load unrelated modules or progressively add modules during normal execution.** If newly discovered scope genuinely triggers another module, load it only when necessary. If that new scope also changes the required execution tier or materially changes the workstream, use the restart rule in Section 4.

- `modules/UI-UX.md` — new/materially changed user-facing workflow, screen, control, navigation, form, or interaction state.
- `modules/SECURITY-AUTH.md` — authn/authz, permissions, tenancy, secrets, tokens, security boundaries, abuse controls.
- `modules/DATA-MIGRATIONS.md` — schema, migrations, persistence contracts, durable record lifecycle, bulk data change.
- `modules/PERFORMANCE.md` — performance/scale work or latency-sensitive data loading/navigation/rendering.
- `modules/PRODUCTION-EXTERNAL-SYSTEMS.md` — deployment, credentials, production mutation, external providers, webhooks, watches, polling, provider reconciliation.
- `modules/LONG-SPEC-TRANSFER.md` — large/chunked requirement transfer or prompt-integrity assembly.
- `Codex-Efficiency-Standards.md` — compute/context audit, unusually large tasks, repeated agent inefficiency, or explicit efficiency tuning.

A project `AGENTS.md` should be a compact routing/enforcement layer, not a duplicate standards manual.

For substantive work, report which modules were loaded in one compact line or in the normal completion report. Do not narrate module-selection reasoning.

## 6. Efficient Execution Cycle

Unless the task requires otherwise:

1. Load core + project instructions + triggered modules once.
2. Establish the smallest practical core tool surface for the run.
3. Confirm repository/branch/state once.
4. Locate the requested implementation surface with targeted search.
5. Inspect the minimum relevant code, tests, callers, and contracts.
6. Check whether requested functionality already exists or is partially implemented.
7. Make the smallest coherent change satisfying acceptance criteria.
8. Run the cheapest high-signal verification first.
9. Expand verification only as risk/change surface requires.
10. Review final diff/status once implementation stabilizes.
11. Commit/push only when requested or required by the task.
12. Stop when acceptance is met.

Reuse established context within the task. Do not repeatedly rediscover repository structure, branch, relevant files, architecture, standards, test commands, or dependency relationships unless state changed or prior evidence is incomplete.

Prefer patching over rewriting. Avoid unrelated refactors, cleanup, renaming, formatting churn, speculative abstraction, and duplicate helpers/tests/features.

### Phased execution + midstream QA/QC

When a coherent story contains dependent implementation phases, prefer one continuous execution run with explicit internal phases and mandatory midstream QA/QC gates. Do not split the story into separate runs solely to validate an intermediate phase.

Default pattern:

1. Implement the prerequisite or foundation phase.
2. Run a midstream QA/QC gate proportional to that phase's risk.
3. On `PASS`, continue immediately into the next phase in the same run.
4. On a remediable `FAIL`, repair defects, rerun the affected gate verification, and continue once it passes.
5. Repeat phase/gate cycles as appropriate.
6. Run a final QA/QC gate before completion.

A midstream gate is an internal checkpoint, not a default stop. Stop only when safe continuation is materially blocked by an unresolved product/business decision, a missing prerequisite, an unsafe architecture conflict, a security or data-integrity issue that cannot be safely remediated within scope, required production/live/external acceptance, scope expansion large enough to invalidate the planned execution, or an execution environment that cannot safely perform the next phase.

Prefer a separate story or run when the next phase materially requires a different model, priority, or execution profile; a human/product decision or live/production/external acceptance is required; the first phase intentionally creates an independently deployable or releasable dependency; the combined story is too large or incoherent for one reliable context; or explicit user instruction requires separate execution. If a materially different execution profile is required, preserve completed work and use the clean-boundary restart rule in Section 4; never claim an in-run model or priority switch that did not occur.

Within a continuous phased run, reuse established repository, standards, architecture, test, and dependency context. Avoid duplicate documentation, repeated broad regression without material changes, and unnecessary commit/push boundaries. This is an efficiency rule, not a relaxation of QA, correctness, security, or required execution resources.

## 7. Verification

Implementation and verification are distinct. Never claim a requirement works merely because implementing code exists.

Use proportional verification:

1. syntax/static validation on changed files;
2. direct unit/regression tests;
3. related subsystem tests;
4. build/type-check/lint where applicable;
5. full suite only when change surface, isolation, project gates, or release requirements justify it.

Run inexpensive checks before expensive checks. Do not rerun a passing check unless relevant code changed afterward, another fix could affect it, or final acceptance explicitly requires a fresh run.

For a midstream gate, select checks proportional to the phase's risk, such as focused automated tests, architecture or contract-invariant review, security/auth/tenant-isolation tests, migration/data-integrity checks, concurrency/idempotency checks, rendered UI or accessibility validation, adversarial cases, or provider/external-system contract tests. Where the environment supports it and risk justifies it, use a fresh reviewer/subagent or independent adversarial pass; do not require extra reviewers ceremonially for low-risk work.

Midstream gates do not replace final verification. Before declaring the complete story `PASS`, verify integrated behavior across phases, run the proportional final regression required by the total change surface, review final diff/status, and satisfy repository or project release gates.

Use truthful statuses where useful: `PASS`, `FAIL`, `NOT RUN`, `NOT APPLICABLE`.

Never claim rendered QA, live-provider verification, physical-device testing, deployment, remote state, or production activation unless actually performed.

## 8. Scope, Failure, and Stop Rules

- Do not silently reinterpret material ambiguity.
- Do not substitute similar behavior for explicitly required behavior without reporting the discrepancy.
- Do not broaden a defect-remediation story into a refactor.
- If blocked by missing credentials, unavailable services, ambiguous requirements, environment limits, or external infrastructure, make a reasonable targeted attempt, preserve completed work, report the blocker, and stop spinning.
- Do not repeat identical failed operations without changed conditions.
- Stop implementation when acceptance is met and required verification passes.

## 9. Git / Release Discipline

Before commit, inspect final status/diff, confirm no unrelated files are modified, and exclude temporary/debug artifacts.

When push is requested, push the intended branch and verify remote success when practical. Do not report commit/push/remote verification unless actually performed.

Production-intended functionality follows the project's normal mainline path. Experimental functionality follows documented beta/experimental rules. Repository-specific branch/release rules take precedence.

## 10. Durable Documentation

Important product requirements, architecture decisions, contracts, security requirements, deployment procedures, and reusable instructions should live in the repository rather than only in chat.

Do not create documentation merely for ceremony. Update durable docs when the change materially alters a contract, architecture, operational procedure, or reusable rule.

## 11. Completion Report

Keep completion reports concise and decision-useful. Include only applicable items:
- what changed;
- verification performed/results;
- branch/commit/push state;
- loaded modules;
- blockers, known limitations, or deferred scope.

For a phased story, briefly identify completed phases, each midstream gate result, gate remediations, the final gate result, and any stop/escalation reason. Keep this compatible with the existing evidence line below.

Do not reproduce the prompt or internal reasoning.

For substantive story work, emit exactly one compact evidence line and no extra telemetry narrative:

```text
EFF {"story":"ID","files":N,"unrelated":N,"verify":"PASS|FAIL","compliance":"PASS|FAIL","commit":"SHA|null","push":"PASS|FAIL|NA","model":null|"X","priority":null|"X"}
```

Set `model` and `priority` to `null` unless the runtime explicitly exposes the values actually used. Never infer token usage, elapsed time, cost, runtime model, or runtime priority.

## 12. Evolution

When a repeatable rule is globally reusable, place it in the smallest appropriate location:
- core only if nearly every task needs it;
- a conditional module if only a task class needs it;
- project `AGENTS.md` if project-specific.

Do not duplicate the same rule across core, modules, and project files. Periodically remove rules whose cost exceeds their demonstrated value.
