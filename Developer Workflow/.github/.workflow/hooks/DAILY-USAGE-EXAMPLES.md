# Daily Usage Examples: Real Scenarios

These are practical, copy-paste examples of how to use hooks in real development.

## Scenario 1: New Feature Friday Morning

**Task:** Implement "add webhook retry logic" feature.

### Step 1: Start with hook
```
Execute hook: hooks/pre-task/context-load
Feature: webhook-retry
```

Agent loads:
- Existing spec (if any)
- Related decision notes
- Applicable patterns

### Step 2: Create requirement
```
Requirement:
- Webhooks should retry up to 3 times
- Exponential backoff: 5s, 25s, 120s
- Retry on 5xx, timeout errors only
- Max 30 minute window per attempt

Acceptance:
- Failed webhooks are retried
- Backoff timing verified
- Non-retriable errors fail immediately
```

**Agent:** Run Requirement Agent → get summary

### Step 3: Save impact snapshot
```
Execute hook: hooks/post-impact/memory-snapshot
Feature: webhook-retry
Impacted files:
  - app/bss_integration_hub/models.py
  - app/bss_integration_hub/service.py
  - tests/bss_integration_hub/test_service.py
Entry points: webhook delivery function
Risk areas: timing logic, database state on retry
```

**Output:** Decision note saved to ai-memory/

### Step 4: Create & approve spec
```
Execute hook: hooks/spec/approval-gate

Spec saved: specs/webhook-retry.md

User review and approval.
```

### Step 5: Implement
```
Execute hook: hooks/implementation/pre-implementation.scope-check

Spec: specs/webhook-retry.md
Target files:
  - app/bss_integration_hub/models.py
  - app/bss_integration_hub/service.py
```

→ Scope validated ✅

```
Execute hook: hooks/implementation/post-implementation.context-update

Then manually run Implementation Agent:
Spec: specs/webhook-retry.md
Target files: [above]
```

### Step 6: Generate tests
```
Execute hook: hooks/implementation/post-implementation.test-generation

Spec: specs/webhook-retry.md
Changed files:
  - app/bss_integration_hub/service.py (added retry logic)
Test framework: tests/bss_integration_hub/
```

Agent outputs test cases for:
- Retry on 5xx
- Retry on timeout
- Backoff timing
- Max 3 attempts
- Non-retriable errors

### Step 7: Review before PR
```
Execute hook: hooks/review/pre-review.spec-check
Spec: specs/webhook-retry.md
Diff: [paste git diff]

→ Validates alignment ✅

Execute hook: hooks/quality/review-diff
Spec: specs/webhook-retry.md
Diff: [git diff]
Tests: 12/12 passing ✅
```

### Step 8: Archive findings & create PR
```
Execute hook: hooks/review/post-review.findings-archive

Then create PR and run:
Execute hook: hooks/pr/post-pr.changelog-update
```

**Total time: 2 hours | Total tokens: ~3800**

---

## Scenario 2: Bug Fix Wednesday Afternoon

**Bug:** Webhooks hang on network timeout instead of failing fast.

### Step 1: Debug
```
Execute hook: hooks/debug/post-debug.memory-archive

Symptom: Webhook delivery stalls for 5+ minutes
Logs: [paste relevant error]
Code snippet: [webhook handler function]
```

Agent outputs:
- Root cause: Missing timeout on HTTP client
- Fix: Add 30-second timeout to requests
- Test: Add timeout test case

### Step 2: Apply fix
```
After implementing fix, run:
Execute hook: hooks/implementation/post-implementation.test-generation

Changed files:
  - app/bss_integration_hub/service.py (added timeout)
Test framework: tests/bss_integration_hub/
```

Agent generates:
- Test for timeout behavior
- Test for timeout + retry interaction

### Step 3: Review & PR
```
Execute hook: hooks/review/pre-review.spec-check

Spec: specs/webhook-retry.md (existing)
Diff: [git diff]

→ Validates this fix maintains spec compliance ✅

Execute hook: hooks/quality/review-diff

Then create hotfix PR.
```

**Total time: 45 minutes | Total tokens: ~1500**

---

## Scenario 3: Refactoring Thursday (Optimize Payment Service)

**Task:** Extract payment retry logic into reusable module.

### Step 1: Analyze impact
```
Execute hook: hooks/pre-task/context-load
Feature: extract-retry-module
```

### Step 2: Map refactor scope
```
Execute hook: hooks/post-impact/memory-snapshot
Feature: extract-retry-module
Impacted files:
  - app/bss_copilot/service.py (uses retry)
  - app/bss_integration_hub/service.py (uses retry)
  - app/common/utils/ (new location)
```

### Step 3: Plan refactor (no new spec needed, behavior unchanged)
```
Manually run Design Agent:
Objective: Extract shared retry logic

Agent outputs: Proposed module structure
```

### Step 4: Implement
```
Execute hook: hooks/implementation/post-implementation.context-update

Then manually refactor code.
```

### Step 5: Verify no behavior change
```
Execute hook: hooks/implementation/post-implementation.test-generation
Changed files:
  - app/common/utils/retry.py (new)
  - app/bss_copilot/service.py (uses new module)
  - app/bss_integration_hub/service.py (uses new module)

Agent generates regression tests to verify original behavior preserved.
```

### Step 6: Code review for safety
```
Execute hook: hooks/quality/review-diff
Diff: [git diff]

Verify: No logic changes, only structure.

Then save pattern if reusable:
Execute hook: hooks/context/update-patterns
Pattern: retry-module-pattern
Example files: app/common/utils/retry.py
```

### Step 7: Create refactor PR
```
Execute hook: hooks/pr/pre-pr.gate-readiness
```

**Total time: 3 hours | Total tokens: ~2800**

---

## Scenario 4: API Change Monday (Add JWT Signing)

**Task:** Add JWT signing to authentication API (breaking change).

### Step 1: Context load
```
Execute hook: hooks/pre-task/context-load
Feature: add-jwt-signing
```

### Step 2: Create spec (with migration plan)
```
Execute hook: hooks/spec/approval-gate

In spec, document:
- Old contract: POST /auth → returns session cookie
- New contract: POST /auth → returns JWT + session cookie
- Migration path: Clients must switch to JWT by [DATE]
- Deprecation: Session cookies removed in [VERSION]
```

**GATE:** User approves migration plan ✅

### Step 3: Check for breaking changes
```
Execute hook: hooks/quality/detect-breaking-changes
Spec: specs/add-jwt-signing.md
Diff: [git diff with API changes]

Agent flags: Breaking change detected
Requires: Migration window, client updates
```

### Step 4: Implement
```
Execute hook: hooks/implementation/pre-implementation.scope-check
Spec: specs/add-jwt-signing.md
Target files:
  - app/bss_copilot/api.py
  - app/bss_copilot/service.py
```

→ Scope validated ✅

Then implement.

### Step 5: Add backward compatibility tests
```
Execute hook: hooks/implementation/post-implementation.test-generation
Spec: specs/add-jwt-signing.md

Agent generates:
- Tests for new JWT signing path
- Tests for old session cookie path (still works)
- Tests for both tokens returned
```

### Step 6: Review with migration focus
```
Execute hook: hooks/quality/detect-breaking-changes
Diff: [git diff]

Verify: Migration plan documented
Verify: Deprecation timeline clear
Verify: Old clients still work
```

### Step 7: Create migration PR
```
Execute hook: hooks/pr/post-pr.changelog-update

In changelog, note:
- Added: JWT signing option
- Changed: Auth API now returns both JWT and session
- Deprecated: Session cookies (will be removed in v2.0)
- Migration: See specs/add-jwt-signing.md
```

**Total time: 4 hours | Total tokens: ~3600**

---

## Quick Command Reference

| Task | Command |
|---|---|
| Start new feature | `Execute hook: pre-task/context-load` |
| Save impact analysis | `Execute hook: post-impact/memory-snapshot` |
| Approve spec | `Execute hook: spec/approval-gate` |
| Check scope | `Execute hook: implementation/pre-implementation.scope-check` |
| Generate tests | `Execute hook: implementation/post-implementation.test-generation` |
| Review code | `Execute hook: quality/review-diff` |
| Check breaking changes | `Execute hook: quality/detect-breaking-changes` |
| Save bug info | `Execute hook: debug/post-debug.memory-archive` |
| Pre-PR checklist | `Execute hook: pr/pre-pr.gate-readiness` |
| Update changelog | `Execute hook: pr/post-pr.changelog-update` |
| Extract pattern | `Execute hook: context/update-patterns` |
