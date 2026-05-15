# Hook: post-spec.risk-analysis

## Purpose
Generate a prioritized risk register and testing priorities.

## Prompt Template

```text
HOOK: Post-Spec Risk Analysis

Inputs:
- Spec path
- Impact summary
- Known bug/security history (if available)

Output:
- Risks by severity
- Risk to validation mapping
- Release blocking conditions
- Gate signal: RISK_ACCEPTED or RISK_REVIEW_REQUIRED

Max output: 300 tokens.
```
