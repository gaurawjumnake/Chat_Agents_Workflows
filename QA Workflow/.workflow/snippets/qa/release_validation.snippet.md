# release_validation.snippet.md

## Use
Decide release readiness from QA gate outputs.

## Prompt
```text
Perform release validation.
Input:
- Smoke results: <summary>
- Regression result: <summary>
- Security gate: <pass/block>
- Open defects: <list>
Output:
1) Go/No-Go
2) Blocking issues
3) Minimum follow-up actions
Keep findings first.
```
