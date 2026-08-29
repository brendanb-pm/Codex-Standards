# UI / UX Module

Load only for new or materially changed user-facing workflows, screens, controls, navigation, forms, or interaction states.

## Required states
Handle applicable ready, loading, success, empty, validation-error, permission-denied, recoverable-failure, uncertain-outcome, retry/recovery, cancellation/back-navigation, session-expiration, stale-data, concurrency, and version-conflict states.

Failed or cancelled operations must return to a deterministic stable state. Avoid stale loading indicators, duplicate submissions, orphaned state, misleading success, and corrupted session/tenant context. Preserve entered data across recoverable failures when safe. Do not blindly replay a consequential mutation with an uncertain result; reconcile authoritative state first.

Error messages must be understandable and actionable without exposing stack traces, persistence details, provider internals, secrets, or security metadata.

## Human factors
Do not require users to remember, copy, transcribe, infer, or reconstruct information the system already possesses and can safely present contextually.

Prefer human-readable contextual selection over raw internal IDs. Keep known IDs as optional power-user search when useful. Bounded lists should be relevant and sensibly ordered; use server-side search/pagination for large datasets.

For materially changed workflows, audit routine inputs for avoidable operator-memory dependence and classify findings as `FIXED`, `JUSTIFIED POWER-USER INPUT`, or `DEFERRED WITH REASON`.

## Interaction quality
Evaluate primary goal/action, information hierarchy, system status, validation, recovery, duplicate-submit prevention, touch usability, keyboard/focus behavior, accessibility, responsiveness, operator-readable language, and technical-detail leakage.

Unless project-specific device requirements supersede them, consider:
- desktop ~1440x900
- tablet landscape ~1024x768
- tablet portrait ~768x1024
- mobile ~390x844

Check overflow, clipping, overlap, dialog fit, hidden required information, touch-target size, form usability, action wrapping, and readability.

## Evidence
Never infer rendered/visual QA from source review, unit tests, DOM inspection, CSS rules, or static analysis.

When applicable, report separately:
- `CODE / FUNCTIONAL STATUS`
- `UI/UX CODE-LEVEL QA`
- `RENDERED VISUAL QA`
- `PERFORMANCE / RESPONSIVENESS QA`

Call a workflow UI/UX complete only when successful behavior is usable, applicable loading/empty/validation/permission states are handled, expected failures recover to a stable state, retry/recovery is safe, and applicable behavior has actually been verified.

Also load `modules/PERFORMANCE.md` when the work changes latency-sensitive navigation, data loading, search, rendering, or external calls.
