# Hook: quality.review-diff

## Purpose
Run focused code review on diff.

## Trigger
Pre-review spec check passed.

## Input
- Spec path
- Git diff
- Test results summary

## Prompt Template (Copy-Paste)

```
HOOK: Quality Code Review Diff

Spec: specs/<feature-name>.md
Diff: <paste git diff>
Tests: <pass/fail/coverage %>

Review against spec for:
1. Correctness: Does code implement spec correctly?
2. Regression risk: Are existing behaviors preserved?
3. Edge cases: Are spec edge cases handled?
4. Tests: Are spec test scenarios covered?
5. Error handling: Are failures handled per spec?

Output format:
- FINDINGS (by severity):
  - [HIGH] <issue>
  - [MED] <issue>
  - [LOW] <suggestion>
- REQUIRED_FIXES: <must fix before merge>
- MERGE_READY: Yes/No

Focus on spec alignment. No style-only comments.
```

## Expected Output
- Findings ordered by severity
- Merge readiness decision

## Token Rules
- Analyze diff only
- Reference spec by path
- Max output 400 tokens
