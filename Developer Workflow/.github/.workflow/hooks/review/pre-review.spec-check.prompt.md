# Hook: pre-review.spec-check

## Purpose
Validate that diff aligns with spec before running code review.

## Trigger
Before running review, when diff is ready.

## Input
- Spec path
- Git diff

## Prompt Template (Copy-Paste)

```
HOOK: Pre-Review Spec Check

Spec: specs/<feature-name>.md
Diff: <paste git diff>

Quick validation:
1. Does diff implement all spec sections? ✅ or ⚠️
2. Are there changes outside spec scope? ✅ or ⚠️
3. Are API contracts matched in diff? ✅ or ⚠️

Output:
- ✅ SPEC_ALIGNED: Safe to review
- ⚠️ SPEC_MISMATCH: List deviations
- ❌ OUT_OF_SCOPE: Flag extra changes

If NOT aligned, stop and clarify with implementer before review.
```

## Expected Output
- Spec alignment validation
- Proceed/Stop signal

## Token Rules
- Compare spec + diff only
- No code analysis beyond scope
- Max output 150 tokens
