# Autonomous Engineering Production Readiness Charter

**Document:** Production Readiness Charter  
**Path:** `Codex-Standards/Autonomous Engineering/PRODUCTION-READINESS-CHARTER.md`  
**Version:** 1.0.0  
**Status:** IMMUTABLE GOVERNANCE BASELINE  
**Authority:** Product Owner  
**Story Prefix:** `AE`

---

## 1. Purpose

This Charter defines the non-negotiable safety, authorization, verification, recovery, publication, and operational requirements for the Autonomous Engineering system.

The Autonomous Engineering system exists to perform software-development work with minimal human intervention while preserving engineering rigor, authorization boundaries, auditability, recoverability, and independent verification.

The intended mature operating model is:

**Product Owner / governing requirements**
→ **Hermes orchestration**
→ **Codex primary implementation**
→ **deterministic validation**
→ **independent verification**
→ **controlled publication**
→ **terminal CI qualification**
→ **next authorized work**

The system may improve itself, but it may not redefine the criteria by which its own safety or production readiness is judged.

---

# 2. Immutability

Once version 1.0.0 of this Charter is:

1. committed to the authoritative Codex Standards repository,
2. assigned an exact Git SHA,
3. cryptographically hashed,
4. and explicitly ratified by the Product Owner,

the contents of this version are immutable.

The original file must never be silently rewritten, replaced, weakened, or retroactively reinterpreted.

No autonomous agent, including Hermes, Codex, a verifier, supervisor, recovery process, or future autonomous component, may amend this Charter under delegated autonomous authority.

## 2.1 Amendments

Changes require explicit Product Owner authority.

An amendment must:

- exist as a separate versioned artifact;
- identify the Charter version being amended;
- state exactly what is being changed;
- explain why;
- preserve the original Charter;
- record Product Owner authorization;
- receive its own Git identity and cryptographic hash;
- preserve the complete amendment history.

Autonomous agents may **recommend** Charter amendments.

They may not approve them.

No amendment may be inferred from implementation behavior, historical precedent, prior recovery authority, or a desire to obtain PASS.

---

# 3. Story Namespace

All stories whose primary purpose is to improve the Autonomous Engineering system must use the prefix:

`AE`

Examples:

- `AE-1`
- `AE-1.1`
- `AE-2`
- `AE-17`

Legacy `CP-*` artifacts remain historical records and must not be renamed.

New work moves to the `AE` namespace.

---

# 4. Authority Principle

Authority must always be explicit, bounded, attributable, and fail closed.

No component may create authority that it does not already possess.

Authority for one class of work does not imply authority for another.

Specifically:

- product-story authority does not authorize control-plane modification;
- control-plane authority does not authorize product modification;
- recovery authority does not authorize unrelated implementation;
- parent authority does not automatically authorize child execution;
- historical authorization does not authorize work against a different baseline;
- successful verification does not itself authorize publication;
- publication does not itself authorize deployment;
- autonomous operation does not authorize modification of this Charter.

When authority is ambiguous:

**STOP or ESCALATE.**

Never infer permission.

---

# 5. Fail-Closed Principle

Unknown, unverifiable, inconsistent, corrupted, revoked, mismatched, or ambiguous execution state must fail closed.

The system must never convert uncertainty into success.

Examples include:

- unknown package type;
- invalid seal;
- baseline mismatch;
- unexpected changed files;
- unverifiable CI identity;
- authority mismatch;
- unverifiable recovery state;
- historical artifact mutation;
- verifier failure;
- execution-state corruption.

Fail-closed behavior must preserve useful evidence and recoverable work wherever possible.

---

# 6. Source Authority

Every production-capable Autonomous Engineering component must have an authoritative source identity.

At minimum:

- authoritative Git repository;
- exact baseline SHA;
- exact qualified source SHA;
- traceability from qualified source to installed runtime;
- reproducible dependency definition;
- immutable qualification evidence.

Mutable installed-runtime directories are not authoritative source.

No installed runtime may claim qualification when its contents cannot be traced to an exact independently qualified source revision.

---

# 7. Baseline-First Engineering

Before implementation begins, all mandatory validation gates that can reasonably be executed against the authorized baseline must be evaluated.

The system must establish what was true **before** the story changed anything.

Baseline results must distinguish at minimum:

- PASS;
- comparable pre-existing failure;
- non-comparable pre-existing failure;
- blocked;
- not run.

A story must not be given an impossible acceptance contract because the repository already violates a global rule.

---

# 8. Acceptance Contracts

Acceptance criteria must be generated before implementation.

For each applicable validation gate, the system must use one of:

### ABSOLUTE PASS

Baseline passed.

The post-change gate must pass.

### NO REGRESSION

Baseline already failed in a deterministically comparable manner.

The story must introduce no new failures and must satisfy applicable changed-file requirements.

The global failing condition must continue to be reported truthfully.

### ESCALATION REQUIRED

Baseline state cannot be safely measured or compared.

Implementation must not proceed until explicitly resolved.

Acceptance criteria must not be silently weakened after implementation.

Any post-implementation change to acceptance semantics requires a separately versioned and explicitly authorized amendment preserving the original result.

---

# 9. Primary Execution Model

Codex is the default primary code implementation executor.

Hermes is the orchestration layer.

Hermes implementation fallback is prohibited unless specifically authorized by the execution package.

A failure by Codex is not automatically authorization for Hermes to implement.

The default response to verified Codex compute exhaustion is:

`WAIT_FOR_REFRESH`

not implementation fallback.

---

# 10. Compute Exhaustion

Verified compute exhaustion is an operational condition, not an implementation failure.

The system must support:

`ACTIVE`
→ `COMPUTE_REFRESH_WAIT`
→ `COMPUTE_AVAILABLE`
→ `INTEGRITY_RECOVERY`
→ `RESUME`

A story must preserve:

- authorization;
- worktree;
- changed files;
- execution phase;
- evidence;
- validation state;
- continuation state;
- known refresh information.

Ordinary failures, timeouts, test failures, tool failures, and network failures must not be falsely classified as compute exhaustion.

---

# 11. Recovery

Recoverable failures should be handled automatically when the recovery action:

- remains within existing authority;
- is deterministic or bounded;
- does not weaken verification;
- does not broaden scope;
- preserves audit evidence.

Recovery requiring new authority must escalate.

Normal recoverable engineering defects should not require repeated Product Owner prompts.

The mature system should distinguish at minimum:

- implementation defect;
- test-harness defect;
- baseline debt;
- environment failure;
- CI failure;
- transient/flaky failure;
- compute exhaustion;
- integrity failure;
- authority required;
- unresolved failure.

---

# 12. Persistence

Authorized active execution must survive, where technically supported:

- orchestrator restart;
- controller restart;
- terminal closure;
- logout/login;
- machine reboot;
- compute refresh.

Restart must never cause a completed story to execute again.

Recovery must perform integrity validation before continuation.

---

# 13. Independent Verification

A component must not be the sole authority validating its own implementation.

Required independent verification must be capable of inspecting:

- authoritative source;
- exact diff;
- evidence;
- validation results;
- acceptance criteria;
- relevant execution state.

The independent verifier must use a meaningfully separate execution context.

Long-term production architecture must not depend on access to mutable operator-private runtimes.

Verification failure or inability to verify is not PASS.

---

# 14. Self-Modification

Autonomous Engineering may improve its own implementation only under explicit `AE` authority.

Self-modification must never allow the candidate runtime to destroy the last known qualified runtime.

A production-capable system must maintain a model equivalent to:

`QUALIFIED_CURRENT`

and

`CANDIDATE`

Candidate changes must be:

1. implemented;
2. validated;
3. independently verified;
4. installed as candidate;
5. smoke-tested;
6. promoted only after qualification.

Failure must preserve or restore `QUALIFIED_CURRENT`.

The system must be capable of recovering from a failed self-update without depending on the failed candidate.

---

# 15. Publication Semantics

Publication states must distinguish at minimum:

- verified awaiting publication;
- PR published;
- PR CI pending;
- PR CI passed;
- merge in progress;
- merged awaiting main CI;
- published;
- CI failed;
- publication unresolved.

`PUBLISHED` means terminal success.

It may only be reported when:

1. the implementation passed required verification;
2. required PR CI passed for the exact applicable commit;
3. authorized merge completed;
4. final main SHA is known;
5. required CI for that exact final main SHA reached terminal PASS;
6. final tree integrity was confirmed.

A running post-merge CI job is not `PUBLISHED`.

---

# 16. Exact-SHA Evidence

Evidence from one commit must not qualify another commit.

CI, verification, baseline, publication, and installation evidence must be tied to exact source identity.

Stale successful evidence must never be substituted for evidence required for the current revision.

---

# 17. Auditability

Each autonomous story must leave an evidence trail sufficient to reconstruct:

- authority;
- baseline;
- plan;
- implementation;
- changed files;
- validation;
- recovery actions;
- compute interruptions;
- verification;
- publication;
- final state.

Historical evidence must be append-only or otherwise integrity-protected.

The system must never erase failed attempts merely because a later attempt passes.

---

# 18. Secrets and Credentials

Secrets, credentials, private keys, tokens, authentication artifacts, and private runtime material must not enter source control or qualification evidence unless explicitly designed for secure secret storage.

The verifier must not receive unnecessary access to operator secrets.

Autonomous execution may use only credentials explicitly provisioned for its authorized role.

---

# 19. Kill Capability

The Product Owner must retain a practical means to stop autonomous execution.

The system must honor:

- revoked authority;
- disabled automation;
- explicit stop state;
- emergency shutdown.

A kill action must not require cooperation from the currently executing implementation agent.

Stopping execution should preserve recoverable state when possible.

---

# 20. Concurrency

Until Production Readiness is achieved:

`MAX_ACTIVE_AUTONOMOUS_STORIES = 1`

The MVP must not autonomously execute multiple implementation stories concurrently.

Background observation of CI or compute availability may occur but must not create overlapping implementation authority.

Higher concurrency requires later explicit authorization.

---

# 21. MVP Operating Mode

Before full Production Readiness, the allowed autonomous operating mode is:

`AUTONOMOUS_ENGINEERING_MVP`

MVP mode may autonomously execute qualified `AE` work inside an explicitly approved parent program envelope.

It may not imply global autonomous product-development authority.

During MVP:

- one active implementation story;
- Codex primary executor;
- explicit authority;
- fail-closed operation;
- CP-00-style compute recovery;
- independent verification;
- candidate/current runtime protection;
- global product automation remains disabled.

---

# 22. Autonomous Self-Hardening Program

The first authorized MVP program is:

`AE-1 — Autonomous Engineering Production Readiness`

Its purpose is to bring Autonomous Engineering from MVP capability to the Production Readiness conditions defined by this Charter.

AE-1 may create and execute child `AE` stories only within the explicitly authorized AE-1 program scope.

It may:

- inspect current capabilities;
- identify gaps against this Charter;
- create bounded child stories;
- implement them;
- validate them;
- independently verify them;
- publish qualified control-plane improvements;
- upgrade the candidate runtime;
- promote qualified runtime revisions;
- continue to the next authorized AE-1 story.

AE-1 may not:

- amend this Charter;
- grant itself broader authority;
- enable unrestricted product-story automation;
- change Product Owner policy;
- bypass failed verification;
- lower acceptance standards;
- authorize Hermes coding fallback;
- modify unrelated product software;
- declare Production Readiness solely from unit tests.

---

# 23. Minimum AE-1 Capability Areas

AE-1 must evaluate and, where not already satisfied, address at least:

1. authoritative control-plane source and runtime provenance;
2. control-plane execution-package admission;
3. baseline preflight;
4. pre-implementation acceptance-contract generation;
5. compute exhaustion checkpoint/wait/resume;
6. recovery-package admission;
7. bounded automatic recovery classification;
8. independent verifier isolation;
9. publication exact-SHA qualification;
10. post-merge main-CI terminal handling;
11. self-update candidate/current rollback;
12. supervisor and cleanup robustness;
13. execution telemetry;
14. compute telemetry/headroom representation;
15. runtime health/status reporting;
16. restart/reboot recovery;
17. audit/evidence integrity;
18. kill/revocation behavior;
19. production-readiness proving campaign.

Existing qualified capabilities must be reused rather than rebuilt.

---

# 24. Production Readiness Gate

Autonomous Engineering may be declared `PRODUCTION_READY` only when every mandatory condition below is supported by evidence.

## 24.1 Engineering Controls

- authoritative Git source established;
- installed runtime tied to exact qualified source SHA;
- baseline-first preflight operational;
- acceptance contracts created before implementation;
- baseline debt handled without false PASS;
- exact-SHA CI qualification operational;
- final post-merge CI required before PUBLISHED;
- independent verification operational;
- self-update rollback demonstrated;
- compute exhaustion recovery demonstrated;
- restart/reboot recovery demonstrated;
- bounded recovery classification operational;
- kill/revocation behavior demonstrated;
- immutable audit trail preserved.

## 24.2 Safety Controls

- autonomous agents cannot expand their own authority;
- Charter cannot be autonomously amended;
- invalid/missing authority fails closed;
- verifier failure cannot become PASS;
- failed self-update cannot destroy last qualified runtime;
- product deployment remains separately authorized;
- no unrestricted Hermes implementation fallback;
- no global wildcard authorization;
- no unresolved critical integrity defects.

## 24.3 Autonomous Proving Campaign

At least **three consecutive real, medium-complexity product-development stories** must complete end-to-end under autonomous control.

The proving campaign must collectively demonstrate:

- implementation;
- validation;
- independent verification;
- PR publication;
- PR CI;
- merge;
- final main CI;
- exact-SHA qualification;
- terminal completion.

Across those runs, the campaign must also demonstrate at least:

- one verified compute-exhaustion checkpoint and automatic resume;
- one restart or reboot recovery;
- one bounded nontrivial engineering recovery without Product Owner intervention.

For the three proving stories:

**Human intervention target: zero**, except for genuine authority/safety escalation.

Routine engineering errors must not require Product Owner recovery prompts.

## 24.4 Production Readiness Decision

When all requirements are satisfied, AE-1 must create a:

`PRODUCTION_READINESS_DECISION_PACKAGE`

containing the supporting evidence.

The autonomous system may recommend:

`PRODUCTION_READY`

It may not activate unrestricted production autonomous engineering by itself.

Final production activation requires explicit Product Owner approval.

---

# 25. Production Readiness Is Not Global Deployment Authority

`PRODUCTION_READY` means the Autonomous Engineering execution system is qualified for its approved autonomous-development role.

It does not automatically authorize:

- product production deployment;
- infrastructure destruction;
- credential rotation;
- financial transactions;
- external customer actions;
- policy changes;
- unrestricted concurrency;
- Charter amendments.

Those authorities remain separately governed.

---

# 26. Success Standard

The mature system should make the normal experience:

**one authorization**
→ autonomous execution
→ autonomous bounded recovery
→ compute wait/resume when necessary
→ independent verification
→ publication
→ terminal CI
→ completion

The Product Owner should be interrupted only when human authority, judgment, or policy is genuinely required.

Repeated manual orchestration of ordinary engineering failures is considered a system deficiency to be addressed by `AE` work.

---

# 27. Governing Principle

Autonomy is earned through evidence.

The objective is not to eliminate human authority.

The objective is to eliminate unnecessary human orchestration while preserving engineering rigor, independent verification, auditability, and control.