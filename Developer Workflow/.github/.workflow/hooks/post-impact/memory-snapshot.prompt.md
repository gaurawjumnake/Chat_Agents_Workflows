# Hook: post-impact.memory-snapshot

## Purpose
Save impact analysis to memory so it's never re-scanned.

## Trigger
Impact analysis phase completed.

## Input
- Feature name
- Impacted files list
- Risk hotspots (High/Med/Low)
- Entry points

## Prompt Template (Copy-Paste)

```
HOOK: Post-Impact Memory Snapshot

Save this decision note to .copilot/ai-memory/decisions/

Filename: 2026-05-15-impact-<feature-name>.md

Content:

# Impact: <feature-name>

- Date: 2026-05-15
- Related spec: specs/<feature-name>.md (create after approval)
- Entry points: <list files/functions>
- Impacted files:
  - <file1> (reason)
  - <file2> (reason)
- High-risk areas: <max 3>
- Tests to update: <max 3 file paths>

This note prevents re-scanning. Reference it instead of reanalyzing.
```

## Expected Output
- Saved decision note with filename

## Token Rules
- Use paths, not code
- No full file analysis
- Max output 150 tokens
