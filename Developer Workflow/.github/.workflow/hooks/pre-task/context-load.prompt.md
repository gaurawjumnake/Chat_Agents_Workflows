# Hook: pre-task.context-load

## Purpose
Load all relevant context before starting a task—specs, decisions, patterns.

## Trigger
Task starts and feature name is known.

## Input
- Feature name: (string)
- Optional: related spec path (e.g., `specs/payment-idempotency.md`)

## Prompt Template (Copy-Paste)

```
HOOK: Pre-Task Context Load

Feature: <feature-name>

Action: Load ALL relevant context for this feature.

Search:
1. In .copilot/specs/: any spec matching "<feature-name>"
2. In .copilot/ai-memory/decisions/: any decision notes mentioning this feature
3. In .copilot/ai-memory/patterns/: any pattern notes matching the domain
4. In .copilot/ai-memory/snippets/: any snippet variant notes

Output format:
- Spec path (if exists) + summary
- Relevant decision notes (max 3)
- Applicable patterns (max 3)
- Applicable snippet variants (max 2)

Total output: max 200 tokens.
```

## Expected Output
- Spec file reference or "No spec found"
- Up to 3 decision bullets
- Up to 3 pattern bullets
- Up to 2 snippet references

## Token Rules
- No full code scanning
- Use file paths as references
- Max output 200 tokens
