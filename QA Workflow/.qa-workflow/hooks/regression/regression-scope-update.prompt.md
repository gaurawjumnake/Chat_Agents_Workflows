# Hook: regression-scope-update

## Purpose
Update targeted regression scope using changed modules and dependencies.

## Prompt Template

```text
HOOK: Regression Scope Update

Inputs:
- Changed modules
- Spec path
- Dependency map notes

Output:
- Must-run regression suites
- Must-run smoke tests
- Deferred suites with rationale

Persist summary to qa-context/regression-map.md.
Max output: 250 tokens.
```
