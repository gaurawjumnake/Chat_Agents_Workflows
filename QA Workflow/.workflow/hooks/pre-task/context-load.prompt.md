# Hook: pre-task.context-load

## Purpose
Load spec, QA context, and relevant QA memory notes before validation starts.

## Prompt Template

```text
HOOK: Pre-Task Context Load

Feature: <feature-name>

Load:
1. specs/<feature-name>.md (if available)
2. qa-context/test-strategy.md
3. qa-context/regression-map.md
4. Related notes in qa-memory/bugs, qa-memory/security, qa-memory/healing

Output:
- Spec reference + short summary
- Relevant prior findings (max 5)
- Suggested validation focus areas (max 5)

Max output: 200 tokens.
```
