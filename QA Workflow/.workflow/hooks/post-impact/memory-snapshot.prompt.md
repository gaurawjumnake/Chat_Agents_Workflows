# Hook: post-impact.memory-snapshot

## Purpose
Persist impact analysis so scope is reused without rescanning.

## Prompt Template

```text
HOOK: Post-Impact Memory Snapshot

Save to qa-memory/regression/
Filename: YYYY-MM-DD-impact-<feature-name>.md

Content:
- Date
- Related spec
- Changed modules and endpoints
- Risk hotspots
- Candidate test suites

Output: saved note path.
Max output: 150 tokens.
```
