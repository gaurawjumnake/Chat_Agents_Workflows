# test_generation.snippet.md

## Use
Generate targeted functional and API tests from spec and changed behavior.

## Prompt
```text
Generate QA tests for this change.
Input:
- Spec path: <path>
- Changed modules or diff: <list>
- Test framework path: <path>
Output:
1) Functional test cases
2) API contract tests
3) Scenario -> test mapping
4) Coverage gaps
Rules:
- Changed behavior only
- Reuse existing fixtures
- Keep focused and deterministic
```
