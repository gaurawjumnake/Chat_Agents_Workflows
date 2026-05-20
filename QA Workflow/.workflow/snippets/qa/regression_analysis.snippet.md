# regression_analysis.snippet.md

## Use
Derive targeted regression scope from changed modules and dependency impact.

## Prompt
```text
Run regression scope analysis.
Input:
- Changed modules: <paths>
- Related specs: <paths>
- Dependency/test map notes: <bullets>
Output:
1) Must-run regression suites
2) Recommended smoke tests
3) Deferred suites with rationale
Avoid full regression default.
```
