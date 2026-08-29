# Security / Authentication Module

Load for authentication, authorization, permissions, tenancy, identity, credentials, secrets, tokens, security boundaries, abuse controls, or security-sensitive recovery.

## Authority boundaries
Authentication proves identity; authorization separately determines allowed action and scope. Never allow client/UI state, provider callbacks, external identifiers, or untrusted claims to become application authority without server-side validation.

Enforce tenant/scope boundaries at the authoritative service/data boundary, not only in UI. Use least privilege and deny by default where practical.

## Session / authentication recovery
Failed, cancelled, interrupted, expired, or timed-out authentication must return to a deterministic safe state. Stale state/nonce/PKCE/session artifacts must not authenticate. Failed reauthentication must not corrupt an otherwise valid authoritative session.

Partial or unauthorized sessions must not acquire authority. Provider errors shown to users must not expose sensitive internals.

## Secrets
Never persist raw passwords, API keys, access/refresh tokens, app-specific passwords, or equivalent secrets in source, business records, logs, operator-facing UI, test fixtures, or general documentation.

Use approved secret/credential storage and persist only safe references when needed. Redact sensitive values from logs and reports.

## Mutations and audit
Security-sensitive and permission-changing mutations require appropriate auditability and idempotency. Unknown mutation outcomes must be reconciled before retry.

Do not weaken security checks or valid security tests to make a suite pass. Security defects require regression evidence that would have failed before the fix.

## Verification
Verify both allowed and denied paths appropriate to the change, including tenant/scope isolation where applicable. For auth flows, verify at least one realistic failure/recovery path in addition to success.

Load `modules/PRODUCTION-EXTERNAL-SYSTEMS.md` when credentials, provider configuration, production auth setup, webhooks, or external identity providers are changed. Load `modules/DATA-MIGRATIONS.md` when security/identity schema changes.
