# Hook: pre-implementation.scope-check

## Purpose
Verify implementation scope matches spec before coding.

## Trigger
Implementation is about to start (after spec approval).

## Input
- Spec path
- Target files list

## Prompt Template (Copy-Paste)

```
HOOK: Pre-Implementation Scope Check

Feature spec: specs/<feature-name>.md
Target files: <list>

Validate:
1. All affected files from spec are in target list
2. No extra files added to target
3. Target files are minimal (no full rewrites)

Output:
- ✅ SCOPE_OK: Ready to implement
- ⚠️ SCOPE_MISMATCH: List misalignment
- ❌ SCOPE_CREEP: Extra files detected

If not OK, stop and clarify scope with user.
If OK: Proceed to Implementation Agent with spec path + target files only.
```

## Expected Output
- Scope validation result
- Proceed/Stop signal

## Token Rules
- Compare spec + files only
- No code scanning
- Max output 100 tokens
