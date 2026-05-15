# Hook: post-review.findings-archive

## Purpose
Save code review findings to decision log for future reference.

## Trigger
Code review completed and findings documented.

## Input
- Feature name
- Review findings
- Any deviations from spec

## Prompt Template (Copy-Paste)

```
HOOK: Post-Review Findings Archive

Save to .copilot/ai-memory/decisions/

Filename: 2026-05-15-review-<feature-name>.md

Content:

# Code Review: <feature-name>

- Date: 2026-05-15
- Spec: specs/<feature-name>.md
- Findings:
  - [HIGH] <issue> (FIXED/PENDING)
  - [MED] <issue> (FIXED/PENDING)
- Deviations: <from spec>
- Approved: Yes/No
- Reviewer notes: <short>

This note archives review decision. Link in PR summary.
```

## Expected Output
- Saved review findings note

## Token Rules
- Bullets only
- No code snippets
- Output <= 150 tokens
