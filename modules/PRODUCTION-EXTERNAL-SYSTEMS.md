# Production / External Systems Module

Load for production mutation, deployment, credentials, OAuth/provider configuration, external APIs, calendars, messaging, webhooks, watches, subscriptions, polling, or provider-owned records.

## Production boundary
Do not mutate production resources unless the task explicitly authorizes that specific mutation. Development work, fake providers, tests, documentation, disabled configuration, or staging setup do not imply production activation permission.

Report every production change explicitly.

## External providers
External systems must integrate through documented boundaries/adapters where the project architecture requires them. Provider failure must not corrupt canonical application state.

Handle applicable duplicate delivery, replay, double-submit, timeouts, provider outages, partial failure, and unknown transport outcomes. Use idempotency/correlation/version checks where appropriate.

When mutation success is uncertain, refresh/reconcile authoritative state before retrying. Do not blindly replay consequential operations.

External deletion or lifecycle changes must not silently destroy canonical records unless the domain contract explicitly permits it.

## Polling / scheduled work
Use event-driven or bounded refresh where practical. Avoid polling or scheduled intervals shorter than the useful lifetime of the underlying information without a concrete requirement.

## Credentials
Use approved secret storage. Never commit or expose raw provider secrets/tokens. Keep operator-facing errors free of sensitive provider/security internals.

## Verification
Separate local/fake-provider verification from live-provider verification. Never report deployment, activation, subscription/watch creation, remote mutation, or provider verification unless actually performed.
