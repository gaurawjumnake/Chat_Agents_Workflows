# Hook: post-implementation.context-update

## Purpose
Save implementation decision and updated context to memory.

## Trigger
Implementation completed and code patch generated.

## Input
- Spec path
- Changed file paths
- Git diff (summary or full)

## Prompt Template (Copy-Paste)

```
HOOK: Post-Implementation Context Update

Save to .copilot/ai-memory/decisions/

Filename: 2026-05-15-impl-<feature-name>.md

Content:

# Implementation: <feature-name>

- Date: 2026-05-15
- Spec: specs/<feature-name>.md
- Changed files: <list paths only>
- Key decision: <what was implemented>
- API changes: <contract deltas, if any>
- Dependencies added: <if any>
- Migration notes: <if any>

Next: Run post-implementation.test-generation hook.
```

## Expected Output
- Saved implementation decision note

## Token Rules
- Paths and bullets only
- No code snippets
- Output <= 200 tokens
