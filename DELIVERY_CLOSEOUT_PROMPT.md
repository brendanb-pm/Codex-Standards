# Canonical Delivery Closeout Procedure

Use this executable procedure at the end of every substantive tracked story or sprint. The implementation agent normally performs closeout in the same execution run; do not require a separate agent or session unless governing standards require independent verification.

This procedure operationalizes, but does not replace or redefine:

- `Codex-Standards.md`;
- `modules/DELIVERY-CLOSEOUT.md`;
- canonical EFF v2;
- applicable project `AGENTS.md` or `AGENTS.project.md`;
- triggered security, migration, production/external-system, performance, Agentic Operations, and verification rules.

If this procedure conflicts with a governing standard or explicit current instruction, the higher-authority instruction wins.

## Invocation

Invoke when implementation is believed ready for acceptance/integration or immediately after required integration. Use the approved canonical standards revision resolved by the consumer project's standards handler; do not copy this file into the consumer repository.

Required flow:

`LOAD CURRENT DELIVERY STATE -> VERIFY IMPLEMENTATION -> VERIFY INTEGRATION -> CAPTURE FINAL GIT/CI EVIDENCE -> PERSIST EFF -> UPDATE NOTION -> RE-FETCH CONTROL PLANE -> VERIFY PERSISTENCE -> RECONCILE DEPENDENCIES -> DETERMINE FINAL CLOSEOUT STATE -> REPORT / HANDOFF`

## 1. Establish closeout context

Record or resolve from authoritative evidence:

- project and story/sprint identifier;
- authoritative Notion record;
- repository, worktree, current branch, remote, and current Git state;
- approved canonical standards SHA;
- applicable project instructions and canonical modules;
- existing implementation, verification, integration, CI, and deployment evidence.

Load `Codex-Standards.md` and `modules/DELIVERY-CLOSEOUT.md`, plus only other modules triggered by the work. Confirm that the standards checkout is at the approved SHA.

Do not re-implement the story during closeout except for bounded remediation permitted by scope and the governing standards. If material implementation work remains, return to the normal execution lifecycle and resume closeout only after implementation stabilizes.

## 2. Determine Implementation Acceptance

Verify applicable acceptance criteria from actual evidence, not a previous claim of success. Check only what the story and governing standards require, including as applicable:

- targeted and regression tests;
- typecheck, build, lint, and CI;
- security, authorization, tenancy, and data-integrity gates;
- schema and migration verification;
- runtime, browser, rendered, accessibility, or physical-device evidence;
- performance measurements;
- independent verification;
- production or external-system acceptance.

Do not fabricate or infer missing verification. If a required gate fails, set:

```text
IMPLEMENTATION ACCEPTANCE: FAIL
```

Perform only permitted bounded remediation, rerun affected verification, and reassess. Otherwise report the exact failure or blocker and do not declare the story complete.

### Evidence-backed zero-change completion

Do not require source changes merely to establish delivery. Investigation, measurement, validation, reconciliation, or performance work may complete with zero implementation files changed when applicable acceptance criteria pass and the evidence is retained.

For a no-change performance result, require the evidence defined by `modules/PERFORMANCE.md`, including a passing baseline, no meaningful bottleneck, passing verification, and retained reproducible measurements. Do not manufacture a diff.

## 3. Determine Integration Acceptance

When integration is required, verify from the remote provider and authoritative repository state:

- intended branch and PR;
- required checks and CI result;
- merge state and merge commit, when applicable;
- current remote default/main branch;
- final authoritative main SHA.

Do not report integration from local state alone. If integration is intentionally deferred under a Human Validation Zone or another governing rule, record the reason and remaining acceptance explicitly.

## 4. Capture final delivery evidence

After the required integration state is known, capture the final Git/CI evidence used for reporting and EFF generation. Reconcile conflicting local, PR, CI, remote-main, or Notion values against their governing authorities before writing either control plane.

Git/main, CI, and verified acceptance evidence remain authoritative for implementation delivery. Notion remains authoritative for project status.

## 5. Generate and persist EFF v2

Generate exactly one canonical EFF v2 record using the schema in `Codex-Standards.md` without adding, removing, or renaming fields.

- Populate only directly supported values.
- Set `standards_sha` to the canonical revision that actually governed the run.
- Use null where evidence is unavailable; never infer runtime model/priority, timing, token/compute usage, or verification.
- A successful evidence-backed no-change story may use `"files":0`, `"verify":"PASS"`, and `"compliance":"PASS"` when accurate.

When an approved writable EFF ledger exists:

1. persist the complete record;
2. obtain its durable identity or equivalent evidence;
3. query or re-read it;
4. compare the persisted record with the intended record.

Printing EFF is not persistence. If no approved writable ledger exists, persistence fails, or the write cannot be verified, set `EFF: BLOCKED` and preserve the complete record in the reconciliation handoff. Do not confuse the standards-change-event ledger with an EFF-run ledger.

## 6. Synchronize Notion

Fetch the authoritative Notion story/sprint record immediately before final mutation and reconcile it with final Git and delivery evidence.

Update only applicable supported fields, including:

- final status and PASS/BLOCKED disposition;
- PR number, merge SHA, and final main SHA;
- CI/check result;
- material architecture and product decisions;
- remaining deployment or external acceptance;
- unresolved blockers;
- dependency changes and newly unlocked work.

After writing, re-fetch the record and compare every required value with the intended final values. A successful write response without re-read verification is insufficient.

Set `NOTION: PASS` only after persistence is confirmed. If access, write, or re-fetch verification fails, set `NOTION: BLOCKED`, retain the exact intended updates, and create the reconciliation handoff.

## 7. Reconcile dependencies

After the story's final implementation state is known, inspect applicable dependencies. Determine whether the result:

- satisfies another dependency;
- unlocks another story;
- changes sequencing;
- creates a new blocker; or
- invalidates a prior dependency assumption.

Update and re-fetch the control plane when required. Do not mark dependent work ready while another dependency remains unsatisfied. Report newly unlocked work explicitly.

## 8. Determine the final closeout state

Apply the canonical dual-acceptance model:

### Complete

```text
IMPLEMENTATION ACCEPTANCE: PASS
CONTROL-PLANE ACCEPTANCE: PASS
STORY/SPRINT: COMPLETE
```

Control-Plane Acceptance requires both verified Notion synchronization and verified EFF persistence.

### Implementation complete; control-plane synchronization blocked

```text
IMPLEMENTATION: COMPLETE
NOTION: PASS|BLOCKED
EFF: PASS|BLOCKED
CONTROL PLANE: SYNC BLOCKED
STORY/SPRINT: NOT FULLY CLOSED
```

Do not invalidate or roll back otherwise valid implementation because a control-plane operation is unavailable. Produce the reconciliation handoff below.

### Implementation failed or blocked

```text
IMPLEMENTATION ACCEPTANCE: FAIL
STORY/SPRINT: FAILED|BLOCKED
```

Choose `FAILED` or `BLOCKED` from the actual failure semantics. Never use control-plane failure alone to mark valid implementation failed.

## 9. Reconciliation handoff

When Notion or EFF persistence is blocked, output the complete handoff structure required by `modules/DELIVERY-CLOSEOUT.md`. Populate exact values and field names so another qualified control/orchestration agent can finish without prior conversation history:

```json
{
  "story": {"project":"PROJECT","id":"STORY_OR_SPRINT","implementation_disposition":"COMPLETE|FAILED|BLOCKED"},
  "git": {"pr_number":null,"branch":"BRANCH","merge_sha":null,"final_main_sha":"SHA|null"},
  "verification": {"ci":"PASS|FAIL|BLOCKED|NOT RUN","checks":[],"independent_verification":"PASS|FAIL|NOT APPLICABLE","remaining_deployment_acceptance":[]},
  "notion_required_update": {"target":"NOTION_RECORD","persistence_status":"PASS|BLOCKED","fields":{},"dependency_changes":[],"newly_unlocked":[],"material_decisions":[]},
  "eff_required_update": {"record":"COMPLETE EFF v2 JSON","ledger_destination":null,"persistence_status":"PASS|BLOCKED"},
  "failure": {"operation":"NOTION|EFF","observed_error":"TEXT","retry_safe":null,"uncertain_mutation_requires_reconciliation":false}
}
```

If a mutation outcome is uncertain, reconcile authoritative state before retrying. Do not overwrite newer control-plane state blindly.

## 10. Final report

Return only decision-useful evidence in this structure, omitting non-applicable detail but not required states:

```text
DELIVERY CLOSEOUT

Project:
Story/Sprint:

IMPLEMENTATION
Status:
Changed files:
Verification:
CI:
Architecture/schema/config changes:

INTEGRATION
PR:
Merge:
Final main SHA:

CONTROL PLANE
Notion: PASS|BLOCKED
EFF persistence: PASS|BLOCKED
Control-plane acceptance: PASS|BLOCKED

DEPENDENCIES
Changed:
Newly unlocked:

FINAL DISPOSITION
Implementation: COMPLETE|FAILED|BLOCKED
Control Plane: PASS|SYNC BLOCKED
Story/Sprint: COMPLETE|NOT FULLY CLOSED|FAILED|BLOCKED

BLOCKERS / REMAINING ACCEPTANCE
[only applicable items]

EFF
[exactly one canonical EFF v2 line]
```

When synchronization is blocked, append the reconciliation handoff. Do not reproduce execution history or narrate routine commands.

## Invocation text for execution briefs

Future substantive story briefs should end with an instruction equivalent to:

> After implementation and required integration, execute `DELIVERY_CLOSEOUT_PROMPT.md` from the approved Codex-Standards revision.

Reference this procedure; do not duplicate it in story briefs or consumer repositories.
