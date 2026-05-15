# Hook: quality.review-diff

## Purpose
Run focused QA review on diff with findings-first output.

## Prompt Template

```text
HOOK: Quality Review Diff

Inputs:
- Spec path
- Git diff
- Test and regression summaries

Review for:
1. Correctness vs spec
2. Regression risk
3. Missing tests
4. Security-sensitive behavior

Output:
- Findings by severity
- Required fixes
- Merge readiness

Max output: 400 tokens.
```
