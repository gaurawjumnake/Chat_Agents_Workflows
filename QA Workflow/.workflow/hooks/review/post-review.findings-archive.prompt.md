# Hook: post-review.findings-archive

## Purpose
Persist QA review findings for future regressions and release auditability.

## Prompt Template

```text
HOOK: Post-Review Findings Archive

Save to qa-memory/bugs or qa-memory/regression as applicable.

Content:
- Date
- Spec path
- Findings by severity
- Resolution status
- Release impact

Output: saved note path(s).
Max output: 150 tokens.
```
