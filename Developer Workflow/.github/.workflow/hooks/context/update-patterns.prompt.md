# Hook: context.update-patterns

## Purpose
Extract and save reusable patterns discovered during implementation.

## Trigger
After implementation, if new pattern discovered.

## Input
- Pattern description
- Example code snippet or file path
- Context/use case

## Prompt Template (Copy-Paste)

```
HOOK: Context Update Patterns

Save to .copilot/ai-memory/patterns/

Filename: <pattern-name>.md

Content:

# Pattern: <name>

- Date: 2026-05-15
- Problem: <short problem statement>
- Context: <where is this pattern useful>
- Solution: <steps or pseudocode>
- Example files: <paths>
- Benefits: <why use this pattern>
- Caveats: <limitations>

Next time similar problem appears, reference this pattern instead of rebuilding.
```

## Expected Output
- Saved pattern note

## Token Rules
- Pseudocode or links, not full code
- Problem/solution focused
- Output <= 200 tokens
