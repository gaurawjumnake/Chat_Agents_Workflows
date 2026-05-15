# Hook: post-spec.approval-gate

## Purpose
Hard stop for spec approval before implementation.

## Trigger
Spec drafted and saved to `specs/<feature-name>.md`.

## Input
- Spec file path

## Prompt Template (Copy-Paste)

```
HOOK: Post-Spec Approval Gate

STOP: Implementation cannot begin until spec is approved.

Spec path: specs/<feature-name>.md

Checklist:
- [ ] Spec includes Summary
- [ ] Spec includes Affected files
- [ ] Spec includes API contracts
- [ ] Spec includes Business logic
- [ ] Spec includes Edge cases
- [ ] Spec includes Test scenarios
- [ ] Spec includes Non-goals

Action required:
User must confirm: "✅ APPROVED" or "❌ NEEDS REVISION"

If APPROVED:
- Implementation Agent can proceed
- Reference spec path only (do not restate requirement)

If NEEDS REVISION:
- Request specific section feedback
- Update spec
- Return to approval gate
```

## Expected Output
- Approval or revision request (gates implementation)

## Token Rules
- No code analysis
- Gate enforces gated workflow
- Output <= 150 tokens
