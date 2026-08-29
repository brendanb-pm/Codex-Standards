# Core / Module Refactor Migration Notes

## Structural change

- `Codex-Standards.md` becomes the small always-loaded core.
- `Codex-Efficiency-Standards.md` becomes a conditional efficiency/audit module rather than mandatory context for every task.
- Specialized rules move to `modules/` and load only when triggered.
- Project `AGENTS.md` files should route to modules and preserve only project-specific deltas.

## Model selection change

The previous stateful change-alert convention is removed. Every brief gets one deterministic `MODEL / PRIORITY` selection based on T0-T3. The selection is made once before execution and is not repeatedly debated or reconsidered.

A higher-risk discovery during execution produces one restart boundary only when the current run is no longer safe/reliable; a simpler-than-expected task simply reduces work under the same selected model.

## Prompt compatibility

Older prompts that explicitly reference both root standards remain safe, but new prompts should normally reference only the core plus project `AGENTS.md`; the loader selects conditional modules.

## Intended context reduction

The architecture avoids loading UI/UX, security, migration, production/external, performance, long-spec, and deep-efficiency rules on unrelated tasks. Atlas `AGENTS.md` is reduced to project-specific routing and architectural constraints.
