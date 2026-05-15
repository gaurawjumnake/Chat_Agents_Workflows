# review_diff.snippet.md

## Use
Run focused code review before PR.

## Prompt
```
Review this diff against the spec.
Input:
- Spec path: <path>
- Git diff: <diff>
- Test summary: <short>
Output format:
1) Findings by severity (High/Med/Low)
2) Required fixes
3) Missing tests
4) Merge readiness (Yes/No)
Focus on correctness, regressions, and risk.
```
