# Hook: pre-pr.gate-readiness

## Purpose
Hard gate before PR creation—verify all workflow requirements met.

## Trigger
Before creating PR.

## Input
- Feature name
- Spec path
- Test status
- Review status

## Prompt Template (Copy-Paste)

```
HOOK: Pre-PR Gate Readiness

Feature: <feature-name>

Checklist before PR:
- [ ] Spec created: specs/<feature-name>.md
- [ ] Spec approved
- [ ] Implementation matches spec (verified by pre-review.spec-check)
- [ ] Tests generated and passing
- [ ] Code review completed (quality.review-diff)
- [ ] All findings addressed
- [ ] Decision notes archived (post-implementation, post-review)

Output:
- ✅ ALL_CHECKS_PASS: Safe to create PR
- ❌ MISSING: <list what's missing>

If not all pass: stop and complete checklist.
If all pass: Generate PR summary using hook: pr.auto-summary
```

## Expected Output
- Readiness pass/fail
- Missing items list (if any)

## Token Rules
- Checklist only
- No code analysis
- Output <= 150 tokens
