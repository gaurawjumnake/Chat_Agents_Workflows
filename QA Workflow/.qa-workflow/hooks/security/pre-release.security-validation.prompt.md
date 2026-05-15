# Hook: pre-release.security-validation

## Purpose
Run mandatory pre-release security QA checks.

## Prompt Template

```text
HOOK: Pre-Release Security Validation

Inputs:
- Spec path
- Changed modules/endpoints
- qa-context/security-model.md

Validate:
1. Authentication flows
2. RBAC enforcement
3. API abuse resistance
4. Dependency risk updates
5. Secret exposure risks
6. Threat model updates

Output:
- Findings by severity
- Blocking actions
- Gate result: SECURITY_PASS or SECURITY_BLOCK

Max output: 350 tokens.
```
