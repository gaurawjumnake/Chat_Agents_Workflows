# Hook: post-debug.memory-archive

## Purpose
Save bug root cause and fix to memory library for future debugging.

## Trigger
Bug fixed and regression test added.

## Input
- Feature/file name
- Bug symptom
- Root cause
- Fix applied

## Prompt Template (Copy-Paste)

```
HOOK: Post-Debug Memory Archive

Save to .copilot/ai-memory/bugs/

Filename: 2026-05-15-<bug-key>.md

Content:

# Bug: <key>

- Date: 2026-05-15
- Symptom: <error/failure description>
- Root cause: <short explanation>
- Fix: <brief solution>
- Files changed: <paths>
- Regression test: <path or case name>
- Related spec: specs/<feature>.md (if applicable)
- Prevention: <how to avoid next time>

This note prevents recurring bugs. Reference it if same symptom appears.
```

## Expected Output
- Saved bug archive note

## Token Rules
- Short bullets
- No full code
- Output <= 150 tokens
