# Hook: pre-release.smoke-validation

## Purpose
Verify smoke-critical paths before final release decision.

## Prompt Template

```text
HOOK: Pre-Release Smoke Validation

Inputs:
- Smoke suite summary
- Core user paths
- Environment/build identifier

Output:
- Smoke pass/fail summary
- Critical failures (if any)
- Gate: SMOKE_PASS or SMOKE_BLOCK

Max output: 200 tokens.
```
