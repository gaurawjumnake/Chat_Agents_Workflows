# healing_analysis.snippet.md

## Use
Analyze flaky failures and propose safe healing options.

## Prompt
```text
Run healing analysis.
Input:
- Failure artifacts: <logs/screenshots/traces>
- Flaky history: <short>
- Environment data: <short>
Output:
1) Root cause hypotheses with confidence
2) Healing options and risk
3) Verification plan
4) Human approval checklist before patch
```
