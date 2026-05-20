# Hook: spec.approval-gate

## Purpose
Hard stop before advanced QA validation and release gates.

## Prompt Template

```text
HOOK: Spec Approval Gate

Spec path: specs/<feature-name>.md

Checklist:
- [ ] Summary
- [ ] Affected files/modules
- [ ] API contracts
- [ ] Business logic
- [ ] Edge cases
- [ ] Test scenarios
- [ ] Non-goals

Action:
Return APPROVED or NEEDS_REVISION.
If revision is required, list missing sections.
```
