# Delivery Closeout Module

Load for every substantive tracked story or sprint. It operationalizes the core transactional closeout rule without replacing project-specific release, CI, or product instructions.

## Authority and pre-implementation control-plane read

- Git/main, CI, and verification are authoritative implementation and delivery evidence.
- Notion is the authoritative project-status control plane.
- The approved EFF/AI-delivery ledger is authoritative delivery-performance evidence.
- The product owner is final authority for product scope, priority, and material product decisions.
- The orchestration/control layer owns cross-project analysis, exception reconciliation, standards/trend analysis, and next-work orchestration; it is not routine closeout middleware.

Before substantive work, fetch the tracked story/sprint record and confirm its identifier, status, dependencies, scope, acceptance criteria, and blockers. Reconcile material discrepancies against authoritative Git/main and durable delivery evidence. Do not silently execute against materially stale control-plane state.

If Notion is unavailable but Git/specification evidence safely establishes scope and acceptance, implementation may proceed with the outstanding control-plane reconciliation recorded. If scope, acceptance, or dependencies cannot be established confidently, return exactly `CONTROL-PLANE READ BLOCKED` and stop substantive implementation.

## Closeout sequence

1. Establish Implementation Acceptance from actual acceptance, tests/checks, required independent verification, required integration/merge, and verified remote final state.
2. Generate the required EFF v2 record and persist it to the configured authoritative ledger. Printing it in a report is not persistence. Verify persistence by deterministic read, query, or re-fetch when supported.
3. Update the Notion story/sprint with actual final status, PASS/BLOCKED disposition, PR number, merge/final main SHA, CI result, material architecture/product decisions, remaining deployment acceptance, unresolved blockers, dependency changes, and newly unlocked work.
4. Re-fetch Notion and verify required values persisted. A successful write response alone is insufficient.
5. Report both acceptance states and either full closeout or a reconciliation handoff.

For deterministic timestamps available to the analytics layer, distinguish implementation completion from EFF persistence, Notion synchronization, and full closeout. Do not add mandatory EFF fields or invent timing data.

## Control-plane failure and reconciliation handoff

Do not roll back valid merged implementation because Notion or the EFF ledger is unavailable. Use `CONTROL-PLANE SYNC BLOCKED` and provide a complete, machine- and human-readable handoff that does not depend on conversation history:

```json
{
  "story": {"project":"PROJECT","id":"STORY_OR_SPRINT","implementation_disposition":"COMPLETE"},
  "git": {"pr_number":null,"branch":"BRANCH","merge_sha":null,"final_main_sha":"SHA"},
  "verification": {"ci":"PASS|FAIL|BLOCKED|NOT RUN","checks":[],"independent_verification":"PASS|FAIL|NOT APPLICABLE","remaining_deployment_acceptance":[]},
  "notion_required_update": {"target":"STORY_OR_SPRINT","fields":{},"dependency_changes":[],"newly_unlocked":[],"material_decisions":[]},
  "eff_required_update": {"record":"COMPLETE EFF v2 JSON","ledger_destination":null,"persistence_status":"PASS|BLOCKED"},
  "failure": {"operation":"NOTION|EFF","observed_error":"TEXT","retry_safe":null,"uncertain_mutation_requires_reconciliation":false}
}
```

Populate exact field names and values, not prose placeholders, before handing off. Preserve null only for genuinely unavailable evidence. If a write outcome is uncertain, reconcile authoritative state before retrying.

## Capability requirements and downstream control layer

Do not confuse `telemetry/standards-change-events.json` with an EFF-run ledger. If no approved writable EFF ledger exists, persistence is `BLOCKED`. The smallest coherent next capability is an approved durable EFF-record destination with an authenticated append operation, durable record identity, and deterministic read/query by that identity. If Notion connectivity or write verification is unavailable, Notion synchronization is `BLOCKED`.

After a successful closeout or reconciliation handoff, a vendor-neutral orchestration/control layer may verify project state, reconcile exceptions, inspect dependencies and next work, analyze EFF records by project, standards era, tier, run type, work class, model/priority, and verification requirements, and recommend the next brief or evidence-supported standards change. It must use `INSUFFICIENT DATA`, `EARLY SIGNAL`, and `MEANINGFUL SAMPLE`; before/after correlation is not causal proof. Portfolio interpretation is not an implementation-agent duty unless explicitly requested.
