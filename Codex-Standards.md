# Codex Standards — Core

## 1. Purpose

This file is the **always-loaded core** for Codex-assisted development. It defines only rules that are broadly applicable to nearly every task. Specialized rules live in conditional modules under `modules/` and must be loaded only when their trigger applies.

Durable context has four layers: **GLOBAL** (this canonical standard), **PROJECT** (repository architecture, domain, security, environment, and operating instructions), **TASK** (the current brief), and **PROCEDURE** (reusable model-agnostic playbooks). Higher layers must not unnecessarily duplicate lower layers. Load the minimum authoritative context needed for correct execution and expand only when evidence requires it.

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

At each material execution boundary, reassess scope, complexity, risk, uncertainty, and verification needs without re-litigating routine commands. If work becomes simpler, reduce unnecessary reasoning, inspection, and verification. If new evidence reveals unexpected architecture coupling, security boundaries, migration complexity, concurrency, repeated implementation failure, cross-service scope, ambiguous root cause, broad merge conflict, or a broad regression surface, inspect the affected architecture and apply the restart rule when the launched profile is no longer safe or reliable.

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
- `modules/AGENTIC-OPERATIONS.md` — high-risk authority decisions, recovery/handoff, parallel implementation, reusable procedure design, or new-project initialization.
- `Codex-Efficiency-Standards.md` — context/retrieval policy, compute/context audit, unusually large tasks, repeated agent inefficiency, or explicit efficiency tuning.

A project `AGENTS.md` should be a compact routing/enforcement layer, not a duplicate standards manual.

When a project uses a pinned canonical revision, its handler must identify the canonical repository and approved SHA, resolve that revision before substantive work, keep the canonical content read-only during application execution, and report unavailable or unresolvable standards rather than silently approximating them. Load core plus applicable modules, not the entire manual.

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

This is the `TRIGGER -> EXECUTE -> VERIFY -> AUDIT` lifecycle: establish task, authority, risk, standards, and tier; make the smallest coherent change; prove acceptance and applicable engineering requirements; then retain deterministic evidence, EFF v2 telemetry, durable decisions, blockers, and reusable failure knowledge.

Reuse established context within the task. Do not repeatedly rediscover repository structure, branch, relevant files, architecture, standards, test commands, or dependency relationships unless state changed or prior evidence is incomplete.

Prefer patching over rewriting. Avoid unrelated refactors, cleanup, renaming, formatting churn, speculative abstraction, premature generalization, unused extension points, duplicate domain models, and duplicate helpers/tests/features. Implement the smallest solution that completely satisfies current acceptance, architecture, security, reliability, maintainability, and verification; this is not permission for brittle hacks or architectural violations.

Do not narrate routine commands or obvious intermediate steps unless they provide a decision, blocker, safety issue, debugging detail, verification evidence, or audit evidence. Completion reporting must remain concise without losing required EFF v2 output or material evidence.

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

Where practical, prefer an executable deterministic check that produces objective evidence over an agent's prose judgment. Prose acceptance remains valid for behavior that cannot reasonably be automated.

The implementer's assertion is not completion evidence. Apply independent verification proportionally: self-verification may be sufficient for low risk; independent verification is recommended when materially useful for medium risk; and is required when practical and materially applicable for high-risk security, authority, tenancy, destructive, migration, financial/business-integrity, major architecture, or consequential production work. An independent verifier may be a qualified agent or deterministic verification system. For sufficiently risky work, separate implementation from verification/integration acceptance; all generated code follows the same gates.

For a midstream gate, select checks proportional to the phase's risk, such as focused automated tests, architecture or contract-invariant review, security/auth/tenant-isolation tests, migration/data-integrity checks, concurrency/idempotency checks, rendered UI or accessibility validation, adversarial cases, or provider/external-system contract tests. Where the environment supports it and risk justifies it, use a fresh reviewer/subagent or independent adversarial pass; do not require extra reviewers ceremonially for low-risk work.

Midstream gates do not replace final verification. Before declaring the complete story `PASS`, verify integrated behavior across phases, run the proportional final regression required by the total change surface, review final diff/status, and satisfy repository or project release gates.

Use truthful statuses where useful: `PASS`, `FAIL`, `NOT RUN`, `NOT APPLICABLE`.

For overall execution outcomes, use equivalent truthful semantics for `COMPLETE`, `ENVIRONMENT BLOCKED`, and `FAILED`. Environment-blocked work is not fully verified merely because environment-independent checks passed.

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

Important product requirements, architecture decisions, contracts, security requirements, deployment procedures, reusable instructions, and critical system behavior should live in durable repository sources rather than only in chat or proprietary model/session memory. A qualified future engineer or agent must be able to reconstruct critical behavior from code, tests, instructions, ADRs, architecture/domain documents, specifications, standards, or tracked decision records. Conversation history may assist execution but must not become required infrastructure.

Do not create documentation merely for ceremony. Update durable docs when the change materially alters a contract, architecture, operational procedure, or reusable rule.

## 11. EFF v2 — Delivery Telemetry

For every substantive implementation, remediation, or verification run, emit
exactly one compact machine-readable `EFF` line at completion.

EFF v2 separates:
- routing intent;
- runtime evidence;
- deterministic repository/timing evidence; and
- later outcome evidence.

Use:

EFF {"v":2,"project":"PROJECT","story":"ID","attempt":1,"run_type":"Initial|Correction|Remediation|Verification","tier":"T0|T1|T2|T3","requested_model":null|"X","requested_priority":null|"Low|Medium|High","runtime_model":null|"X","runtime_priority":null|"X","standards_sha":"SHA|null","started_at":"ISO-8601|null","ended_at":"ISO-8601|null","elapsed_sec":N|null,"base_sha":"SHA|null","final_sha":"SHA|null","files":N|null,"insertions":N|null,"deletions":N|null,"unrelated":N|null,"verify":"PASS|FAIL|BLOCKED","verification_failures":N|null,"compliance":"PASS|FAIL|PARTIAL","commit":"SHA|null","push":"PASS|FAIL|NA","blocker":null|"description"}

Rules:

1. `project` and `story` identify the durable work unit.
2. `attempt=1` with `run_type=Initial` is the first implementation attempt.
3. Corrections, remediations, and independent verification retain the same story
   identifier and increment `attempt`.
4. `tier` records the execution tier selected under Section 4.
5. `requested_model` and `requested_priority` record routing intent when explicitly
   selected before execution.
6. `runtime_model` and `runtime_priority` MUST remain null unless explicitly
   exposed by the execution environment.
7. `started_at` and `ended_at` must come from deterministic execution/harness
   timestamps when available. Never estimate elapsed duration from model reasoning.
8. `elapsed_sec` is derived from those timestamps, not self-estimated.
9. `base_sha`, `final_sha`, file count, insertions, deletions, and unrelated-file
   count should come from repository evidence when available.
10. Verification reports only checks actually performed.
11. `verification_failures` counts failed verification gates/check executions
    during that attempt when deterministically observable.
12. Compliance reports PASS, FAIL, or PARTIAL against the standards actually loaded.
13. Tokens and compute cost are NOT part of the mandatory EFF line unless a future
    runtime reliably exposes them. Never infer either.
14. PR opened-to-merge time is integration latency and must be tracked separately;
    never describe it as development or coding cycle time.
15. Later outcomes — escaped defects, eventual rework, rollback/revert, production
    acceptance — are appended by orchestration/Git evidence after the run rather
    than fabricated at completion.
16. Use null for unavailable evidence. Absence of evidence is not zero.
17. The EFF line is telemetry, not a second narrative completion report.

ChatGPT/orchestration may combine the EFF line with prompt metadata, Git/CI evidence,
PR metadata, and later acceptance outcomes and write the resulting record to the
cross-project AI Delivery metrics ledger.

### Standards Change Events and eras

`telemetry/standards-change-events.json` is the append-only, machine-readable ledger of material standards changes. EFF v2 remains unchanged: `EFF.standards_sha` is the authoritative per-run technical provenance, and analytics enriches it by resolving that SHA to an event and standards era.

Create an event only for a change plausibly capable of altering measured execution behavior or outcomes, such as model routing, verification, context strategy, autonomy, lifecycle, recovery, Definition-of-Done, telemetry methodology, parallel-agent, or major security/governance rules. Do not create events for typo, formatting, comment, link, or other non-semantic cleanup; Git SHA remains sufficient provenance.

An event records `change_id`, `effective_date`, `previous_standards_sha`, `new_standards_sha`, `title`, `summary`, `change_categories`, `affected_measurements`, and `notes`. Its era begins at `new_standards_sha` and includes descendant minor revisions until a later event defines a new era. A SHA equal to `previous_standards_sha` remains in the prior/baseline cohort. This is deterministic Git-provenance enrichment; do not rewrite historical EFF records or fabricate missing telemetry.

Before/after results are observational, not causal proof. When data permits, segment or control by project, tier, run type, work class, requested model/priority, and materially different verification requirements. Compare PRE versus POST cohorts for attempts/story, correction/remediation rates, verification failures, PASS/FAIL/BLOCKED distribution, unrelated/files changed, deterministic elapsed time, tier distribution, compliance, and supported later rework or escaped defects.

Use the maturity labels `INSUFFICIENT DATA`, `EARLY SIGNAL`, and `MEANINGFUL SAMPLE`. Do not claim statistical significance or causal improvement from a small post-change sample. Append one record for each future material change; do not change EFF v2 or backfill unsupported fields.

## 12. Evolution

When a repeatable rule is globally reusable, place it in the smallest appropriate location:
- core only if nearly every task needs it;
- a conditional module if only a task class needs it;
- project `AGENTS.md` if project-specific.

Do not duplicate the same rule across core, modules, and project files. Periodically remove rules whose cost exceeds their demonstrated value.

Version-control reusable, model-agnostic procedures only when recurring work benefits from them. Use a coherent project location such as existing modules, `playbooks/`, or `skills/`; do not create taxonomy-only files. Candidate procedures include story validation, Definition-of-Done verification, integration or merge review, security review, migration verification, release readiness, and context recovery.
