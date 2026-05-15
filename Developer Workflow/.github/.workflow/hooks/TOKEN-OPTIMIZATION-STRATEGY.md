# Hook Token Optimization Strategy

Hooks are designed to minimize tokens by reducing context reloading and avoiding full repo scans.

## Key Principles

### 1. Persistent Memory Over Repeated Scanning
**Instead of:**
```
Analyze the payment service to find what needs tests.
[Reloads entire payment module = 2000+ tokens]
```

**Use hook:**
```
Execute hook: post-implementation.test-generation
Spec: specs/payment-idempotency.md
Changed files: app/bss_copilot/service.py
[Loads spec + small file list = 400 tokens]
```

**Savings: ~1600 tokens**

### 2. File References Instead of Full Code
**Instead of:**
```
Here's my implementation:
[Pastes 500+ lines of code]
[Now review it for spec alignment]
```

**Use hook:**
```
Execute hook: pre-review.spec-check
Spec: specs/payment-idempotency.md
Diff: [Git diff, not full file]
```

**Savings: ~300 tokens**

### 3. Decision Notes as Searchable Index
**Instead of:**
```
What decisions did we make about this feature?
[Rescans all chat history = 1000+ tokens]
```

**Use hook:**
```
Execute hook: pre-task.context-load
Feature: payment-idempotency
[Loads relevant decision notes from ai-memory/ = 200 tokens]
```

**Savings: ~800 tokens**

### 4. Spec as Single Source of Truth
**Instead of:**
```
Remember, the requirement was...
[Restates requirement from memory]
Now implement...
[Repeats instructions]
```

**Use hook:**
```
Execute hook: implementation
Spec: specs/payment-idempotency.md
Target files: [list]
[Agent loads spec once, uses it for all decisions]
```

**Savings: ~400 tokens per implementation request**

## Token Budget Enforcement

Each hook has a strict token budget. Violating it signals over-contexting:

| Hook | Max Input Tokens | Max Output Tokens | Notes |
|---|---|---|---|
| pre-task.context-load | 300 | 200 | Fast context assembly |
| post-impact.memory-snapshot | 400 | 150 | Save, don't rescan |
| pre-implementation.scope-check | 500 | 100 | Quick validation |
| post-implementation.context-update | 600 | 200 | Decision archival |
| post-implementation.test-generation | 700 | 500 | Code output allowed |
| quality.review-diff | 900 | 400 | Detailed analysis |
| pre-pr.gate-readiness | 700 | 150 | Checklist only |

**Total session token budget (new-feature chain): ~4000 tokens**

Compare to: Traditional ad-hoc workflow = 8000-12000 tokens for same task.

**Savings: 50-60% reduction**

## How Hooks Save Tokens

### Pattern 1: Memory Prevents Re-scanning
```
Day 1: Run post-impact.memory-snapshot → saves 200 tokens to ai-memory/
Day 5: Run pre-task.context-load → loads saved note instead of rescanning (saves 700 tokens)
```

### Pattern 2: Spec Replaces Repeated Context
```
First mention: "The spec requires idempotency on payment API"
All future mentions: "specs/payment-idempotency.md"
Savings per mention: 150 tokens
```

### Pattern 3: Diffs Replace Full Files
```
Instead of: "Here's my entire service.py file (500 lines)"
Use: Git diff (50 lines)
Savings per interaction: 250 tokens
```

### Pattern 4: Hook Outputs Are Reusable
```
post-implementation.test-generation output → paste into test file
No re-prompting needed
Savings: 200 tokens
```

## Token Auditing

To check if you're respecting hook budgets:

1. Count tokens sent to agent in each hook execution.
2. Compare against CATALOG.md token budget.
3. If exceeding budget: truncate context or split into sub-hooks.

Example:
```
Executing: post-implementation.test-generation
Input: spec (180 tokens) + 2 changed files (220 tokens) = 400 tokens ✅
Output: 8 test functions (350 tokens) ✅
Total: 750 tokens (within 700 budget? No, warn)
Action: Split into 2 smaller test-generation calls
```

## Token Monitoring Checklist

- [ ] Every hook execution respects max token budget
- [ ] Memory notes prevent repo rescanning
- [ ] Specs are referenced, not rewritten
- [ ] Diffs used instead of full files
- [ ] Decision notes loaded before re-analyzing
- [ ] Output size matches expected range
