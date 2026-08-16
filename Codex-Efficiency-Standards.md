# Codex Efficiency Standards

## 1. Purpose

`Codex-Efficiency-Standards.md` supplements `Codex-Standards.md` and defines how Codex should execute work with minimal redundant compute, repository inspection, context processing, file churn, and repeated verification while preserving correctness.

Use both standards for every Codex-assisted project unless an explicit current user instruction or project-specific rule takes precedence.

## 2. Primary Principle

Use the minimum work necessary to reach a reliable result.

Optimize for:

- useful information gained per repository read;
- useful implementation progress per edit;
- useful confidence gained per test;
- minimal redundant analysis;
- minimal unnecessary context processing; and
- minimal repeated tool execution.

Efficiency never overrides correctness, security, explicit acceptance criteria, or required verification.

## 3. Inspect Before Acting

Before changing code, determine the smallest information set required to execute safely.

Prefer targeted inspection of:

- the relevant symbol or file;
- the owning module;
- directly related tests;
- immediate callers/dependencies; and
- applicable project/global standards.

Do not recursively read an entire repository or directory by default.

## 4. Reuse Discovered Context

Within the same task, reuse facts already established. Do not repeatedly rediscover:

- repository structure;
- current branch;
- relevant paths;
- architecture already inspected;
- coding conventions already loaded;
- story requirements already supplied;
- test commands already identified; or
- dependency relationships already confirmed.

Repeat inspection only when the file/state changed, prior evidence was incomplete, new evidence creates doubt, or verification specifically requires it.

## 5. Targeted Search First

Use targeted search before broad exploration.

Search for exact symbols, story identifiers, endpoint names, component names, configuration keys, test names, and error strings.

Broaden exploration only when targeted discovery fails or the architecture is genuinely unknown.

## 6. Progressive Inspection

Expand inspection only as necessary:

1. Relevant symbol/file.
2. Direct dependencies and callers.
3. Related tests.
4. Adjacent subsystem.
5. Broader architecture only when required.

Do not begin with repository-wide architectural analysis for a localized story.

## 7. Avoid Redundant Analysis

Do not repeatedly reconsider decisions already resolved unless new evidence changes them.

Once an approach is consistent with the user request, repository architecture, applicable standards, and acceptance criteria, proceed.

Generate multiple theoretical approaches only when tradeoffs materially affect implementation, architecture is ambiguous, risk warrants comparison, or the user requests options.

## 8. Minimize File Churn

Make the smallest coherent change set satisfying the requested work.

Avoid unrelated:

- refactoring;
- formatting;
- renaming;
- directory reorganization;
- cleanup; and
- replacement of working code.

Every modified file must have a clear relationship to the requested task.

## 9. Patch Before Rewrite

Prefer focused edits over full-file rewrites.

A full rewrite is justified only when safe patching is impractical, the component is explicitly being replaced, or severe structural corruption makes reconstruction necessary.

Preserve unrelated content.

## 10. Proportional Verification

Match verification effort to risk and change surface.

Prefer this progression:

1. Syntax/static validation on changed files.
2. Direct unit tests.
3. Related subsystem tests.
4. Build/type-check/lint where applicable.
5. Full suite only when required.

Do not automatically run the full test suite for every small change unless project standards require it, the change is cross-cutting, isolation is poor, or a release gate explicitly requires it.

## 11. Fail Fast

Run inexpensive, high-signal checks before expensive checks.

Examples:

- syntax before full build;
- focused unit tests before integration suites;
- targeted type checks before whole-application builds; and
- configuration validation before deployment simulation.

If an early check fails, fix it before spending compute on downstream checks that depend on it.

## 12. Do Not Re-run Passing Checks Without Cause

After a check passes, do not rerun it unless relevant code changed afterward, another fix could affect it, or final acceptance explicitly requires a fresh run.

Repeated execution that adds no new confidence is waste.

## 13. Batch Related Operations

When safe, group related work:

- inspect related files together;
- make logically related edits in one pass;
- run related focused tests together; and
- perform final status/diff review after implementation stabilizes.

Avoid excessive edit-test-edit-test loops when changes can safely be grouped.

## 14. Use Existing Tests and Utilities First

Before creating a new test, determine whether existing tests already cover the behavior or can be extended meaningfully.

Before adding helpers, validators, adapters, wrappers, logging utilities, retry logic, authorization checks, or parsers, search for an existing equivalent.

Reuse established project abstractions unless there is a concrete reason not to.

## 15. Load Standards Once Per Task

At task start, load applicable global and project standards. Use them throughout the task.

Do not repeatedly reread unchanged standards during one execution unless a conflict appears, exact wording must be revalidated, or the standards changed.

## 16. Canonical-Reference Prompt Rule

Do not reproduce global standards in each Codex prompt when Codex can access the canonical repository.

Canonical repository:

`https://github.com/brendanb-pm/Codex-Standards`

Canonical files:

- `Codex-Standards.md`
- `Codex-Efficiency-Standards.md`

A normal project prompt should reference these files rather than embedding their contents.

Preferred compact instruction:

```text
Follow the canonical standards in brendanb-pm/Codex-Standards:
- Codex-Standards.md
- Codex-Efficiency-Standards.md
```

If the target environment cannot access the canonical repository, use the project's local synchronized copy if one exists. Only inline the minimum required standards when neither source is accessible.

## 17. Mobile Mode Efficiency

When `M:` Mobile Mode is active under `Codex-Standards.md`, aggressively eliminate redundant prompt content.

Prioritize:

1. Story/work-item identifier.
2. Canonical standards reference.
3. Goal.
4. Relevant repository/spec references.
5. Story-specific scope/constraints not already documented.
6. Story-specific acceptance criteria.
7. Story-specific verification.

Do not repeat global standards already available in the canonical Standards repository.

## 18. Desktop Mode Efficiency

`D:` Desktop Mode allows longer prompts but does not justify duplication.

Include detail where it materially improves execution reliability, while continuing to reference canonical standards instead of reproducing them.

## 19. Check Current State Before Implementing

Before creating or remediating functionality, determine whether it already exists, is partially implemented, was addressed by another story, or is already present on the current branch.

Do not recreate functionality merely because the prompt assumes it is absent.

For defects/remediation:

1. Locate or reproduce evidence of the defect.
2. Inspect current implementation.
3. Identify the smallest supported root cause.
4. Fix that cause.
5. Verify requested behavior.

Avoid speculative rewrites based solely on the reported symptom.

## 20. Stop When Acceptance Is Met

When all requested acceptance criteria are satisfied and required verification passes, stop implementation work.

Do not continue with optional improvements unless explicitly requested.

## 21. Escalate Instead of Spinning

Do not repeatedly retry the same blocked operation.

If progress is blocked by missing credentials, unavailable services, ambiguous requirements, environment limitations, inaccessible dependencies, or failing external infrastructure:

- make a reasonable targeted attempt;
- identify the blocker;
- preserve completed work;
- report the exact limitation; and
- state what remains unverified.

Do not consume cycles on identical retries without changed conditions.

## 22. Expensive Operations

Treat potentially high-cost/high-latency operations carefully, including:

- dependency installation;
- full repository builds;
- large test suites;
- network calls;
- deployment operations;
- large dataset processing; and
- repeated remote/API operations.

Before a materially expensive action, determine:

- what new information it will produce;
- whether that information is required;
- whether a cheaper targeted check can answer first;
- whether the action already passed; and
- whether anything changed that justifies rerunning it.

Skip operations with little or no incremental value.

## 23. Git Efficiency

Typical flow:

1. Confirm branch/state once initially.
2. Implement.
3. Inspect final status and diff.
4. Run required verification.
5. Commit when requested.
6. Push when requested.
7. Confirm remote state once.

Additional Git inspection is appropriate only when state changes, conflicts occur, or verification requires it.

## 24. Completion Report Efficiency

Completion reports should be concise and decision-useful.

Report:

- what changed;
- verification performed and result;
- commit/push state when applicable; and
- blockers or remaining risk.

Do not reproduce the original prompt, lengthy implementation narrative, or internal reasoning unless requested.

## 25. Compute Escalation Model

### Level 1 — Targeted

Default for small/localized work:

- targeted search;
- limited file reads;
- focused patch; and
- focused tests.

### Level 2 — Subsystem

Use when work spans related components:

- inspect subsystem architecture;
- modify related files; and
- run subsystem-level verification.

### Level 3 — Repository-Wide

Reserve for genuinely cross-cutting work such as framework migrations, global authorization changes, repository-wide schema changes, major dependency upgrades, or broad architectural refactors.

Do not use Level 3 analysis for a localized story.

## 26. Efficiency vs. Confidence

Fewer operations are not automatically more efficient.

Necessary reads, tests, and verification are not waste. The target is to eliminate operations with low incremental value while preserving the evidence needed for a reliable result.

## 27. Default Efficient Execution Cycle

Unless the task requires otherwise:

1. Read applicable standards once.
2. Confirm repository/branch.
3. Locate requested implementation surface.
4. Inspect minimum relevant code/tests.
5. Determine current state and gap.
6. Make the smallest coherent change.
7. Run focused verification.
8. Expand verification only when warranted.
9. Review final diff/status.
10. Commit/push only as requested.
11. Return concise results.

## 28. Anti-Patterns

Avoid:

- repeatedly scanning the entire repository;
- rereading unchanged files;
- rerunning unchanged passing tests;
- full builds for trivial documentation changes;
- full test suites for isolated changes without justification;
- duplicate helpers/tests/features;
- repeated dependency installations;
- repeated remote fetches with no state change;
- rewriting whole files for tiny edits;
- speculative refactoring;
- verbose completion narratives;
- repeating canonical standards inside every prompt; and
- repeatedly retrying unavailable external dependencies.

## 29. Precedence

Use the precedence defined by `Codex-Standards.md`.

Efficiency rules never override explicit user instructions, mandatory project gates, safety/security requirements, acceptance criteria, or required verification.

## 30. Evolution

When a repeatable compute or context inefficiency is discovered, determine whether it is globally reusable. If so, update this file rather than duplicating the rule across project prompts.
