# Hook: quality.detect-breaking-changes

## Purpose
Detect contract and behavior breaks that impact existing clients or tests.

## Prompt Template

```text
HOOK: Detect Breaking Changes

Inputs:
- Spec path
- API/contract diff
- Affected consumers summary

Output:
- Breaking changes list
- Required migration validations
- Block/allow recommendation

Max output: 300 tokens.
```
