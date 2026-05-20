# Hook-Based Execution Model (Instruction Triggers)

Hooks are workflow triggers, not runtime code.

## Pre-Task Hook

Trigger: New task starts.

Actions:
- Load relevant spec from `specs/` if it exists.
- Load related notes from `ai-memory/decisions/` and `ai-memory/patterns/`.
- Build a minimal context packet: objective, constraints, file targets.

Output:
- One short context bundle (<= 250 tokens).

## Post-Impact Hook

Trigger: Impact analysis completed.

Actions:
- Save impacted modules summary in `ai-memory/decisions/DATE-impact-<feature>.md`.
- Include changed interfaces and risk notes.

Output:
- Persisted impact summary note.

## Post-Spec Hook

Trigger: Spec drafted.

Actions:
- Save spec to `specs/<feature-name>.md`.
- Request explicit approval before any design/implementation.

Output:
- Approved or rejected status.

## Pre-Implementation Hook

Trigger: Implementation is about to start.

Actions:
- Load approved spec only (not raw requirements).
- Load relevant pattern notes from `ai-memory/patterns/`.
- Prepare implementation scope list: files/functions to touch.

Output:
- Narrow coding context packet.

## Post-Implementation Hook

Trigger: Code patch complete.

Actions:
- Generate tests from spec scenarios.
- Save newly discovered reusable pattern to `ai-memory/patterns/`.

Output:
- Test files + optional pattern note.

## Post-Debug Hook

Trigger: Bug resolved.

Actions:
- Save root cause and fix in `ai-memory/bugs/DATE-<bug-key>.md`.
- Link affected spec and tests.

Output:
- Reusable bug note.

## Pre-PR Hook

Trigger: PR preparation.

Actions:
- Run `review_diff` snippet on current diff.
- Generate PR summary from spec + diff + tests.

Output:
- Findings list + PR summary.

## Hook Design Rules

- Hooks must reduce repeated prompting.
- Hooks must reference files instead of re-sending long context.
- Hook outputs must stay compact and structured.
