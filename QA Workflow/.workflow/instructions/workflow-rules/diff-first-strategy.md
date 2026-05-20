# Diff-First Analysis Strategy

## Core Principle

**Analyze changes via diffs and impact, never via full repo scans.**

## Why Diff-First

- ✅ Token efficient (diff = 200 tokens vs full file = 1000+ tokens)
- ✅ Focused analysis (ignore unrelated code)
- ✅ Repeatable (same diff always produces same scope)
- ✅ Scalable (works for large repos)

## Diff Formats

### When You Have a Git Diff

```
Request diff:
git diff main..feature-branch -- src/payment/

Include output as:
```diff
--- src/payment/retry.ts
+++ src/payment/retry.ts
@@ -42,3 +50,5 @@
 - old line
+ new line
```
```

### When You Don't Have Git

Manually describe changes:

```
Changed modules:
- src/payment/retry-strategy.ts:
  Added: ExponentialBackoffCalculator class
  Modified: RetryManager.execute() method
  
- src/database/payment-events.ts:
  Added: retry_attempt_count column
  Modified: event_log_insert() stored proc
```

### Diff Size Limits

| Scope | Max Tokens |
|-------|-----------|
| Single file | 100 tokens |
| Multiple files (< 5) | 250 tokens |
| Module delta (5-10 files) | 400 tokens |

If diff > 400 tokens, trim to critical sections only.

## Analysis Process

### Step 1: Load Diff

```
Diff: [include diff here, max 250 tokens]
```

### Step 2: Map Impact

```
From diff, impacted:
- Functions: [list]
- APIs: [list]
- Data models: [list]
- Dependencies: [list]
```

### Step 3: Identify Test Scope

```
Tests to update:
- Unit tests for RetryManager
- Integration tests for payment events
- Regression tests for payment flow

Tests to add:
- ExponentialBackoff unit tests
- Retry timeout scenarios
```

### Step 4: Skip Full Code Review

❌ DON'T:
- Review entire retry.ts file
- Read all payment module code
- Scan entire codebase for impact

✅ DO:
- Analyze only the diff lines
- Follow import statements to affected modules
- Reference qa-context/regression-map.md for known impacts

## When Full File Context is Needed

Sometimes you need full file context:

**Condition 1**: Logic is too complex to understand from diff alone
**Condition 2**: Diff references changes in 10+ other files
**Condition 3**: Refactoring spans entire module

When needed:

```
Request: Full file context for src/payment/retry-strategy.ts

Justify: "Need to understand entire retry loop to identify 
         edge cases for exponential backoff logic"

Max: ONE file per request, keep to 500 tokens
```

## Diff-to-Test-Scope Mapping

This is the key diff-first pattern:

```
Diff lines 42-50: Added ExponentialBackoffCalculator
  → Test scope: Unit tests for calculator
  → Test scope: Integration with retry loop
  → Regression: Payment retry E2E tests

Diff lines 60-65: Modified retry count logic
  → Test scope: Boundary tests (0, 1, 2, 3+ retries)
  → Test scope: Failure exhaustion
  → Regression: Timeout after retries exhausted

DB Schema change: Added retry_attempt_count
  → Test scope: Migration tests
  → Test scope: Backward compatibility
  → Regression: Event log queries
```

## Preventing Scope Creep

When diff mentions "related code", check if it's actually changed:

```
Diff mentions: PaymentGateway class (not in diff)
Action: Skip PaymentGateway analysis
Reason: Not in scope unless diff includes it

If PaymentGateway behavior is affected:
Action: Ask for explicit diff change to PaymentGateway
Rule: "Analyze changes, not implications"
```

## Regression Scope from Diff

Use this pattern to scope regression:

```
Changed modules: src/payment/retry-strategy.ts

Affected tests (from qa-context/regression-map.md):
- test/payment/retry-timeout.spec.ts
- test/payment/exponential-backoff.spec.ts
- test/e2e/payment-flow.spec.ts

Skip tests (not affected by this diff):
- test/payment/gateway.spec.ts
- test/payment/settlement.spec.ts
- test/billing/invoice.spec.ts
```

## Diff-First Approval Gate

Before moving to next phase:

```
Gate: DIFF_SCOPE_APPROVED

Confirmed:
✓ Diff is complete (all changed files included)
✓ Impact analysis is accurate
✓ Test scope is minimal and correct
✓ Regression scope is limited to affected modules

Proceed: YES → next phase
```

## Tools for Diff Generation

- IDE: Right-click file → "Compare with..." or use Git panel
- Terminal: `git diff main..feature`
- GitHub: PR diff view
- Local comparison: Save before/after, use diff tool

## When Diff Isn't Available

Use "module-first" analysis:

```
No diff available, so analyzing by modules:

Changed modules:
1. src/payment/retry-strategy.ts
2. src/database/payment-events.ts

Assumptions (verify with team):
- Retry backoff algorithm updated
- Event logging schema changed
- No payment gateway changes

If assumptions are wrong, request correct module list.
```
