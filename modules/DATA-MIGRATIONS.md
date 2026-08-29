# Data / Migrations Module

Load for schema changes, migrations, persistence-provider contracts, durable record lifecycle changes, bulk data mutation, or compatibility changes affecting stored data.

## Data integrity
Preserve canonical IDs, relationships, versions, ownership, and audit history unless the explicit requirement changes them.

Prefer additive, backward-compatible migration paths. Define legacy-record behavior and mixed-version behavior when relevant.

Do not bulk-rewrite or destructively transform production data without explicit authorization.

## Migration contract
For material persistence changes, define:
- target schema/contract;
- forward migration order;
- compatibility window or cutover behavior;
- validation checks;
- rollback/disable/recovery path where practical;
- handling of partial failure and restart;
- idempotency/reentrancy when migration can be retried.

Unknown mutation outcomes must be reconciled against authoritative state before retry.

## Canonical state
External systems must not silently become source of truth for canonical business records unless the domain contract explicitly says so. Preserve application state when external changes are ambiguous, destructive, or incomplete; reconcile rather than silently overwrite.

## Verification
Test schema/contract behavior, representative legacy data, and failure/restart behavior proportional to risk. A migration is not verified merely because generation or application code exists.

For high-risk production migrations, also load `modules/PRODUCTION-EXTERNAL-SYSTEMS.md`.
