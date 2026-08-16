# Codex Standards

## 1. Purpose

`Codex-Standards.md` is the canonical cross-project standard for:

- ChatGPT-to-Codex prompts;
- software implementation handoffs;
- coding-agent execution;
- verification;
- story traceability;
- repository documentation;
- branch and release discipline;
- long-specification transfer; and
- failure-mode prevention.

It is written for both humans and AI coding agents. Project-specific rules may extend it under the precedence model below.

## 2. Story and Work-Item Header

Whenever a prompt corresponds to a numbered story, issue, ticket, requirement, or work item, line 1 of the prompt must contain only its identifier, written exactly as tracked by the project. Do not add `Story:`, `Ticket:`, `#`, or any similar prefix. A blank line must follow the identifier.

```text
MOS-121

GOAL
...
```

Other valid first lines include `NEXUS-014` and `ATLAS-087`. A prompt with no associated identifier begins directly with `GOAL`.

## 3. Codex Execution Brief

Use this structure by default:

```text
[WORK-ITEM-ID when applicable]

GOAL

CONTEXT

SCOPE

CONSTRAINTS

ACCEPTANCE

VERIFY

OUTPUT
```

- **GOAL:** Required end state.
- **CONTEXT:** Execution-relevant background only.
- **SCOPE:** Allowed change surface.
- **CONSTRAINTS:** Hard architectural, behavioral, branch, compatibility, security, or exclusion rules.
- **ACCEPTANCE:** Objectively testable completion criteria.
- **VERIFY:** Checks Codex must actually perform.
- **OUTPUT:** Concise completion report Codex must return.

## 4. ChatGPT and Codex Responsibilities

The default division of labor is:

**ChatGPT**

- architecture;
- requirements and tradeoff analysis;
- story development;
- failure-mode analysis;
- prompt construction; and
- broader reasoning.

**Codex**

- repository inspection;
- implementation and focused code changes;
- tests and local validation;
- commits and pushes when requested; and
- execution reporting.

Keep Codex prompts execution-oriented rather than conversational.

## 5. Mobile and Desktop Prompt Modes

Prompt mode is mandatory, stateful behavior for ChatGPT when generating Codex prompts.

### `M:` — Mobile Mode

When a user begins a prompt request with `M:`, ChatGPT must enter **MOBILE PROMPT MODE**. This indicates practical mobile copy/paste character limits.

While Mobile Mode is active:

- make Codex prompts as compact as reasonably possible;
- preserve every implementation-critical requirement;
- remove unnecessary explanation, repeated context, examples, and prose;
- prefer dense structured instructions;
- prefer references to repository-resident standards and specifications over repetition;
- never omit constraints, acceptance criteria, or verification requirements merely to shorten a prompt;
- reference the file and path when a large requirement already exists in the repository;
- optimize for reliable execution per copied character;
- use chunking when a requirement genuinely cannot be compressed safely; and
- retain the mandatory story or work-item identifier when applicable.

`M:` changes the persistent prompt-generation mode. All subsequent Codex prompt requests remain Mobile Mode optimized, even when they omit `M:`, until the user explicitly switches modes with `D:`.

### `D:` — Desktop Mode

When a user begins a prompt request with `D:`, ChatGPT must enter **DESKTOP PROMPT MODE**. This indicates no meaningful copy/paste character limit.

While Desktop Mode is active:

- prompts may be as long as needed for reliable execution;
- include useful full context, detailed constraints, edge cases, acceptance criteria, and verification requirements;
- do not artificially compress requirements; and
- continue to exclude irrelevant conversation and needless duplication.

`D:` changes the persistent prompt-generation mode. All subsequent Codex prompt requests remain in Desktop Mode until the user explicitly switches modes with `M:`.

### Mode Switching and Prefix Treatment

The prefixes are state changes:

- `M:` switches to persistent Mobile Mode.
- `D:` switches to persistent Desktop Mode.

Example sequence:

1. `M: Give me the Codex prompt for MOS-121.` switches to Mobile Mode.
2. `Create a prompt to fix the quote UI.` remains Mobile Mode optimized.
3. `D: Give me a prompt for the next sprint.` switches to Desktop Mode, which then persists until another `M:`.

`M:` and `D:` are ChatGPT prompt-generation control prefixes. They normally must not appear in the resulting Codex prompt; they control how ChatGPT constructs the handoff.

## 6. Prompt Design Principles

- Preserve requirements before prose.
- State outcomes clearly.
- Separate requirements from implementation suggestions.
- Identify hard constraints explicitly.
- Define testable acceptance criteria.
- Always define verification behavior.
- Prevent accidental scope expansion.
- Prefer repository references over repeated large instruction blocks.
- Do not silently reinterpret materially ambiguous requirements; surface them for resolution.
- Do not substitute similar behavior for specifically required behavior without reporting the discrepancy.
- Avoid unrelated refactors during focused story implementation.

## 7. Long Specifications and Chunk Transfer

For a specification too large for a reliable single transfer, use explicit, unique chunk markers:

```text
NEXUS-0B-01-BEGIN

[content]

NEXUS-0B-01-END
```

Requirements:

- number chunks sequentially;
- use a unique identifier for every chunk;
- provide matching `BEGIN` and `END` markers;
- require Codex to validate completeness before writing or executing; and
- stop execution when any chunk is missing, duplicated, malformed, out of order, or incomplete.

Important assembled specifications must end with an integrity marker, for example:

```text
END-NEXUS-SPRINT-0B-SPEC
```

After assembly, verify that:

- every chunk exists;
- chunk order is correct;
- all markers match;
- no content was dropped;
- the integrity marker exists;
- the target file exists; and
- the resulting file contains the expected assembled content.

## 8. Verification Standard

Implementation and verification are distinct. The existence of implementing code or documentation does not prove that a requirement works or that its content is correct.

Use these statuses as applicable:

- `PASS` — performed and satisfied;
- `FAIL` — performed and not satisfied;
- `NOT RUN` — applicable but not performed; and
- `NOT APPLICABLE` — not relevant to the work.

Completion reports must identify applicable items such as:

- files created and modified;
- tests run and their results;
- lint, type-check, and build results;
- acceptance-criteria results;
- branch;
- commit SHA;
- push result;
- remote verification;
- assumptions and known limitations; and
- incomplete requested work.

Never claim a requirement is verified merely because its implementation exists. Never report a test or remote check as performed unless it was actually performed.

## 9. Git and Diff Discipline

Before committing:

- inspect `git status`;
- review the final diff;
- confirm that no unrelated files were modified;
- exclude temporary and debug files; and
- confirm that all requested documentation and code exists.

When a push is requested:

- commit intentionally with a clear, scoped message;
- push the intended branch;
- confirm push success; and
- when practical, verify that the remote repository reflects the intended commit.

## 10. Branch and Release Discipline

- Production-intended functionality follows the project's normal mainline development path.
- Experimental, exploratory, prototype, or explicitly beta functionality follows the project's documented beta or experimental path.
- A feature is not beta merely because it is new.
- Repository-specific branch standards take precedence over this general rule.
- Codex must surface a material branch or release-channel conflict and must not silently implement work in a different channel.

## 11. Durable Repository Documentation

Important development knowledge must live in the repository rather than only in chat. Durable artifacts include:

- product requirements documents and sprint specifications;
- entity-relationship diagrams and architecture decision records;
- architecture documentation;
- API contracts and data-model documentation;
- security requirements;
- deployment and verification procedures; and
- reusable Codex instructions.

Chat is a development interface, not the sole durable source of truth.

## 12. Failure-Mode Guardrails

Explicitly prevent these failures:

- acting on incomplete prompt chunks;
- accepting missing or mismatched `BEGIN`/`END` markers;
- implementing before validating transferred input;
- losing requirements during prompt compression;
- using the wrong branch or release channel;
- expanding scope without a request;
- performing unrelated refactors;
- claiming tests were run when they were not;
- claiming remote push verification without performing it;
- losing traceability between a story and its implementation;
- silently resolving material ambiguities;
- overwriting existing documentation without reviewing it; and
- assuming file creation proves content correctness.

When a guardrail fails, stop or limit execution as appropriate, report the condition accurately, and request clarification when the requirement cannot be resolved safely.

## 13. Standards Precedence

Apply instructions in this order:

1. Explicit current user instruction.
2. Project-specific repository requirements.
3. `Codex-Standards.md`.
4. Default Codex behavior.

A project may tighten this global standard but should not silently weaken it. Surface conflicts between project requirements and this standard.

## 14. Evolution of the Standard

When a repeatable ChatGPT/Codex failure mode or improved development practice is discovered:

- determine whether it is project-specific or globally reusable;
- add globally reusable practices to `Codex-Standards.md`;
- keep project-specific rules in the applicable project; and
- avoid contradictory duplicated versions of global rules across repositories.

## 15. UX Failure, Recovery, and Stable-State Standard

Every user-facing workflow must account for both successful operation and expected failure and recovery behavior. A workflow is not UI/UX complete merely because its happy path works.

Consider and verify these states when applicable:

- initial or ready;
- loading or in progress;
- success;
- empty or no results;
- validation error;
- permission or access denied;
- recoverable or transient failure;
- uncertain outcome;
- retry or recovery;
- cancellation or back-navigation;
- session or authentication expiration; and
- stale data, concurrency, or version conflict.

Apply these failure and recovery principles:

- Failed or cancelled operations must leave the application in a deterministic, stable state.
- Failures must not leave stale loading indicators, partial sessions, duplicate submissions, orphaned UI state, misleading success indicators, or invalid authorization or tenant context.
- Users must be able to retry safely without an unnecessary browser or page refresh when retry is appropriate.
- Retry must not duplicate consequential mutations. Use idempotency, authoritative refresh, or reconciliation where appropriate.
- Preserve entered user data across recoverable failures when safe and useful.
- Do not blindly replay a mutation with an uncertain outcome. Refresh or reconcile authoritative state before deciding whether to retry it.
- Error messages must be actionable and understandable without exposing sensitive security, infrastructure, persistence, stack-trace, or provider internals.
- Alternate valid actions should remain available after failure where practical.
- Repeated failures must not accumulate duplicate sessions, callbacks, commands, or mutations.

Authentication and provider workflows must explicitly ensure that:

- failed authentication returns to a deterministic safe state;
- cancelled authentication returns to a deterministic safe state;
- interrupted or timed-out authentication returns to a deterministic safe state;
- stale callback, state, nonce, PKCE, or session artifacts cannot authenticate;
- fresh authentication can be attempted without an unnecessary browser refresh;
- failed reauthentication does not corrupt an otherwise valid authoritative session;
- partial or unauthorized sessions cannot acquire application authority; and
- provider errors shown to users do not expose sensitive provider or security internals.

Consequential workflows must have at least one realistic failure-and-recovery verification path in addition to happy-path testing.

### UI/UX Completion Rule

A user-facing workflow must not be reported as **UI/UX COMPLETE**, **ACCEPTED**, or equivalent unless:

1. successful behavior is usable;
2. applicable loading, empty, validation, and permission states are handled;
3. expected failure modes return to a stable state;
4. retry and recovery behavior is defined and safe; and
5. applicable failure and recovery behavior has actually been verified rather than inferred from implementation.

Rendered, live-runtime, physical-device, accessibility, and provider-specific evidence must remain separately classified when applicable. Do not imply that any such verification was performed when it was not.

## 16. Human-Factors and Contextual-Interaction Standard

Do not require users to remember, copy, transcribe, infer, or manually reconstruct information that the system already possesses and can safely present contextually.

- Prefer human-readable contextual selection over raw internal identifiers.
- Known business identifiers may remain available as searchable power-user inputs.
- Do not require users to memorize business identifiers when the system can present eligible records.
- Selection controls must provide enough context to distinguish similar records.
- Appropriate context may include customer, project, description, date, status, owner, or other relevant business attributes.
- Default result lists must be bounded, relevant, and sensibly ordered.
- Use server-side search and pagination for large datasets instead of downloading global directories to the browser.
- Preserve context when moving between related workflows.
- Do not make users re-enter information the application already knows.
- Do not expose persistence identifiers, storage schema, security metadata, or implementation details as routine business inputs unless genuinely required.
- Destructive, consequential, or ambiguous actions must communicate their expected effects and provide appropriate confirmation and recovery without unnecessary confirmation fatigue.

Evaluate workflow design from the operator's perspective:

- What are they trying to accomplish?
- What information do they reasonably know?
- What does the system already know?
- What can fail?
- How does the user recover?

### Human-Factors Acceptance Rule

For every new or materially changed user-facing workflow, explicitly audit routine inputs and interactions for avoidable operator-memory dependence. Classify each finding as:

- **FIXED**;
- **JUSTIFIED POWER-USER INPUT**; or
- **DEFERRED WITH REASON**.

A workflow with material avoidable operator-memory dependence must not be reported as fully UI/UX complete merely because its underlying operation works.
