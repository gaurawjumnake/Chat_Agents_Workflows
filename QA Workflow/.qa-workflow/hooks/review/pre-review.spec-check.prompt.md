# Hook: pre-review.spec-check

## Purpose
Validate that changed behavior and tests align with spec before detailed QA review.

## Prompt Template

```text
HOOK: Pre-Review Spec Check

Spec: specs/<feature-name>.md
Diff summary: <summary>

Output:
- SPEC_ALIGNED or SPEC_MISMATCH
- Out-of-scope changes (if any)
- Missing scenario coverage (if any)

Max output: 150 tokens.
```
