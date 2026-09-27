# Bounded Autonomous Delivery Module

Load only for explicitly authorized unattended Level 2 or Level 3 story delivery, an autonomy eligibility decision, or autonomy qualification. This module is provider-, model-, and orchestrator-neutral. It supplements `AGENTIC-OPERATIONS.md`, `DELIVERY-CLOSEOUT.md`, and applicable project instructions; it never grants Product Owner authority or weakens Human Validation Zones.

## Authority separation

- The **Product Owner/authorization authority** owns priority, business intent, acceptance criteria, risk appetite, and consequential approvals.
- The **product/control-plane layer** turns that authority into a bounded package: story, scope, dependencies, constraints, acceptance gates, routing/tier, verification, Human Validation Zones, and stop/escalation conditions.
- **Design/UI specialists** supply authorized direction or artifacts; they do not redefine product requirements.
- The **implementation agent** implements the package, tests/measures, and produces evidence. It may identify ambiguity or recommend change, but may not redefine requirements or acceptance criteria to obtain `PASS`.
- The **orchestrator/operator** enforces state transitions and scope, invokes applicable specialists, coordinates independent verification, tracks/persists evidence, permits only bounded retry, and stops/escalates when authority or capability is insufficient. It owns workflow enforcement, not product authority.

Operator absence never increases scope, permissions, retry budget, product authority, architectural authority, or Human Validation Zone authority.

## Terminal outcomes

Every explicitly authorized unattended execution ends in exactly one terminal outcome; indefinite continued execution is prohibited.

- `PASS` — authorized scope is complete; acceptance is independently verified as required; required evidence is persisted; and delivery/closeout reached the maximum state permitted by the authorization.
- `BLOCKED` — a requirement, dependency, environment, credential, control-plane, verification, or other execution blocker prevents safe completion. Preserve valid work/state, name the blocker precisely, avoid speculative workarounds outside authorization, and provide deterministic recovery information when available.
- `ESCALATION REQUIRED` — evidence shows that safe continuation needs authority, capability, judgment, model capacity, specialist expertise, or risk acceptance beyond the package. Stop further consequential work, preserve completed valid work, identify the exact reason and required decision/capability, and do not broaden scope or authority silently.

`CONTROL-PLANE SYNC BLOCKED` remains the applicable Delivery Closeout state and maps to `BLOCKED` for this module. A `PASS` does not override an unmet delivery-closeout requirement.

## Autonomy ladder

| Level | Authority |
| --- | --- |
| 0 — Shadow | Proposes sequence/actions; human remains execution and approval authority. |
| 1 — Supervised | Executes approved workflow portions and stops at defined consequential boundaries. |
| 2 — Bounded Autonomous | For one explicitly authorized eligible story, may implement, verify, commit/push, and prepare/create a PR under governing standards. Merge and other consequential actions remain subject to Human Validation Zones and explicit authorization. |
| 3 — Delivery Autonomous | For qualified low/medium-risk work, may execute the authorized lifecycle through implementation, independent verification, protected integration/CI, permitted merge, evidence/control-plane synchronization, and Delivery Closeout without continuous supervision. It remains bounded to that story. |
| 4 — Portfolio Autonomous | May select subsequent eligible work only from a separately authorized queue. This level is defined, not approved or default; it requires separate explicit authorization and qualification. |

Progressive autonomy means demonstrated authority to operate safely, not merely greater technical capability.

## Eligibility gate

Before unattended Level 2 or Level 3 execution, the orchestrator must make and persist an explicit determination:

`AUTONOMY ELIGIBLE` or `AUTONOMY INELIGIBLE`, with reasons and evidence.

Eligibility requires applicable evidence that:

1. authoritative requirements and sufficiently deterministic acceptance criteria exist;
2. dependencies are known and satisfied;
3. repository/project identity, applicable canonical revision, and permitted scope are known;
4. required verification infrastructure and relevant rollback/recovery behavior are understood;
5. required credentials/capabilities are available without unauthorized intervention;
6. applicable Human Validation Zones and consequential boundaries are identified;
7. no unresolved consequential product, architecture, security, or risk decision remains; and
8. bounded retry plus stop/escalation conditions are defined.

The implementation agent cannot relax this gate to begin work. Material ambiguity, unapproved external writes, scope expansion, unavailable credentials, insufficient capability, or a triggered Human Validation Zone makes the story ineligible or requires escalation. Existing security, migration, production/external-system, and routing rules remain authoritative; this list is not exhaustive.

## Execution, independent acceptance, and bounded retry

Use the core `TRIGGER -> EXECUTE -> VERIFY -> AUDIT` lifecycle, applicable modules, and `DELIVERY-CLOSEOUT.md`. For autonomous delivery, independent acceptance must be proportional to risk and must not rely only on implementation-agent self-report. Where practical, use a clean checkout/worktree; reconstruct acceptance from authoritative story/control-plane evidence rather than an implementation summary; run independent tests/benchmarks; inspect the diff against authorized scope; and read back persisted evidence. Higher-risk eligible work should use a separate verification invocation/context when practical.

Retry only when the failure is understood sufficiently to make retry safe, remains in authorized scope, does not cross a Human Validation Zone, and reconciles uncertain mutations first. Set a package-appropriate retry limit and escalation condition; do not prescribe a universal numeric limit. Repeated unsuccessful remediation becomes `BLOCKED` or `ESCALATION REQUIRED`, never an uncontrolled loop.

If evidence shows an assigned model, agent, tool, or specialist is insufficient, preserve work/evidence and identify the deficiency. Use a higher-capability route only when it is already authorized by routing, compute, and risk boundaries; otherwise return `ESCALATION REQUIRED`. Do not silently switch to materially higher-cost or higher-authority execution.

## Qualification, measurement, and recovery

Qualification to a higher level is evidence-based and considers both successful unattended delivery and safe stopping. Relevant evidence includes independently verified delivery without intervention, intervention frequency, unsafe/unbounded actions, scope violations, verification failures, bounded-retry behavior, correct `BLOCKED`/`ESCALATION REQUIRED` outcomes, control-plane/evidence persistence, and recovery quality after interruption or failure. Do not grant higher autonomy solely because several stories passed, and do not fabricate unavailable measurements.

The primary objective is: *How many authorized eligible stories reach independently verified delivery without human intervention, and when they cannot, how reliably does execution stop safely before causing harm or unauthorized change?* Existing EFF/control-plane evidence can support story/run identity, routing when exposed, verification/compliance, repository delta, blockers, and supported closeout evidence. It does not by itself require or reliably expose eligibility disposition, authorization-package identity, human-intervention count, terminal-outcome classification, independent-verification provenance, or all safe-stop details. Record those gaps in durable control-plane, handoff, or reconciliation evidence; do not alter EFF v2 or backfill unsupported data.

For a returning human, extend the existing Context Recovery, Material Agent Handoff, and Delivery Closeout/reconciliation contracts with the authorized package, executed/changed state, verification and persistence evidence, current Git/control-plane state, terminal outcome, remaining action, and whether retry/resume is safe. Do not create a competing recovery format.
