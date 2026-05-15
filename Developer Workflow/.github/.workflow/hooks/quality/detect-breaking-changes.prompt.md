# Hook: quality.detect-breaking-changes

## Purpose
Flag any breaking API changes before PR.

## Trigger
Before review, when API changes detected.

## Input
- Spec path
- Git diff
- Affected API endpoints or functions

## Prompt Template (Copy-Paste)

```
HOOK: Quality Detect Breaking Changes

Spec: specs/<feature-name>.md
Diff: <paste git diff>

Analyze for breaking changes:
1. API request/response contract changes
2. Function signature changes
3. Database schema changes
4. Behavior changes to existing features
5. Removed endpoints/functions

Output:
- BREAKING_CHANGES: <list>
  - <change 1> (requires migration)
  - <change 2> (requires deprecation)
- NON_BREAKING: <list>
- MIGRATION_PLAN: <required steps>

If breaking changes exist, update spec "Non-goals" section
to document migration path. Stop PR until migration planned.
```

## Expected Output
- Breaking changes list
- Migration plan (if any)
- Proceed/Stop signal

## Token Rules
- API surface only
- Reference affected files
- Max output 300 tokens
