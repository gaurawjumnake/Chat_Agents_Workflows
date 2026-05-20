# Hook: post-implementation.generate-tests

## Purpose
Generate targeted QA tests from spec and changed behavior.

## Prompt Template

```text
HOOK: Post-Implementation Generate Tests

Spec: specs/<feature-name>.md
Changed modules/diff: <paths or summary>
Test framework path: <path>

Generate:
1. Functional tests
2. API contract tests
3. Edge and negative tests
4. Regression guard tests

Output:
- Suggested test files
- Scenario -> test mapping
- Missing coverage

Max output: 500 tokens.
```
