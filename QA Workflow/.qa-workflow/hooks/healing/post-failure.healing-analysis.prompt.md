# Hook: post-failure.healing-analysis

## Purpose
Analyze flaky/unstable failures and produce guarded healing recommendations.

## Prompt Template

```text
HOOK: Post-Failure Healing Analysis

Inputs:
- Failure artifacts (trimmed)
- Test history and environment summary
- Related spec path

Output:
1. Root cause analysis with confidence
2. Healing options (selector/timing/schema/env)
3. Risk and rollback plan
4. Human approval requirement

Gate:
- Return HEALING_APPROVAL_REQUIRED before any patch action.

Max output: 300 tokens.
```
