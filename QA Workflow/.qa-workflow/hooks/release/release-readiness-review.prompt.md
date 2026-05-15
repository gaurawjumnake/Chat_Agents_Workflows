# Hook: release-readiness-review

## Purpose
Final go/no-go gate for release.

## Prompt Template

```text
HOOK: Release Readiness Review

Inputs:
- Risk status
- Regression summary
- Security gate result
- Smoke validation result
- Open defects

Output:
- GO or NO_GO
- Blocking issues list
- Required follow-up actions

If NO_GO, do not proceed until blockers are resolved.
Max output: 200 tokens.
```
