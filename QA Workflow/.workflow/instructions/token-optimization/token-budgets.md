# Token Budgets and Allocation Strategy

## Global Token Budget Rules

| Workflow | Max Input | Max Output | Notes |
|----------|-----------|-----------|-------|
| Single Hook | 800 | 400 | Depends on phase, see below |
| Single Snippet | 600 | 300 | Reusable, focused prompts |
| Agent Deep-Dive | 1000 | 500 | When snippet/hook isn't enough |
| Context Snapshot | 300 | 150 | Pre-task context loading |

## Phase-Specific Token Budgets

These are STRICT limits. Split phases if over budget.

| Phase | Input Tokens | Output Tokens | Purpose |
|-------|-------------|---------------|---------|
| 1. Requirement QA | 300 | 200 | Summary only |
| 2. QA Impact | 400 | 150 | Risk hotspots only |
| 3. Spec QA Analysis | 700 | 300 | QA strategy + gaps |
| 4. Risk Analysis | 700 | 300 | Risk register |
| 5. Test Design | 800 | 500 | Test scenarios only |
| 6. Test Execution | 900 | 400 | Failures + root cause |
| 7. Regression | 600 | 250 | Scope only |
| 8. Security | 900 | 350 | Verdict + actions |
| 9. Healing | 700 | 300 | Proposal + guardrails |
| 10. Release | 700 | 200 | Go/No-Go only |

## Input Token Allocation

### Phase 1-2 (High-Level Analysis)
```
Requirement summary:     100 tokens
Entry points:            100 tokens
Prior diff/impact:       100 tokens
────────────────────────────
Total:                   300 tokens
```

### Phase 3-4 (Spec Analysis + Risk)
```
Spec path reference:     100 tokens (metadata only)
Requirement summary:     100 tokens
Current QA strategy:     200 tokens (from qa-context)
Prior risk findings:     200 tokens (from qa-memory)
New changed modules:     100 tokens
────────────────────────────
Total:                   700 tokens
```

### Phase 5-7 (Execution)
```
Spec reference:          100 tokens
Changed modules:         150 tokens
Test framework path:     50 tokens
Existing test patterns:  200 tokens
Failure/logs (critical): 300 tokens (NOT full logs)
Regression context:      150 tokens (from qa-context)
────────────────────────────
Total:                   950 tokens (split if needed)
```

### Phase 8-10 (Security + Release)
```
Spec reference:          100 tokens
Security model:          200 tokens (from qa-context)
Threat findings:         200 tokens (from qa-memory)
Test results summary:    150 tokens
Risk register:           150 tokens
Release gates:           100 tokens
────────────────────────────
Total:                   900 tokens (split by area)
```

## Output Token Strategies

### Summary Output (Phase 1, 2, 10)
- Bullet points only
- One sentence per point
- 150-200 tokens max

### Strategic Output (Phase 3, 4, 7, 9)
- Structured sections
- Evidence references
- Actionable next steps
- 250-350 tokens

### Detailed Output (Phase 5, 6, 8)
- Test cases / scenarios
- Root cause evidence
- Verdict + actions
- 400-500 tokens

## Context Reuse Rules

Before building prompt, check:

1. **Is there a prior hook output?** → Reference it, don't rebuild
2. **Is there a qa-context file?** → Load and reference, don't summarize
3. **Is there a qa-memory note?** → Link and reuse, don't re-analyze
4. **Is this diff available?** → Use diff, not full file
5. **Are test patterns similar?** → Reuse snippets, don't regenerate

### If using prior analysis:
```
(Reference prior finding)
Prior QA analysis: qa-memory/qa-findings/feature-x.md

(New analysis only)
Added analysis: [only new info here]
```

## Log and Artifact Trimming Rules

When including test failures or logs:

❌ WRONG:
```
Full log dump (10KB+)
Entire stack trace
All test output
```

✅ RIGHT:
```
Failed test: test_payment_retry_timeout
Error: timeout after 5s
Stack snippet (last 10 lines):
  at retry-strategy.ts:42
  at promise chain line X
  ...

Relevant logs (last 50 lines):
  [timestamp] ERROR: payment event not found
  [timestamp] WARN: retry bucket exhausted
```

**Rule**: Include only the failing assertion + relevant context (±50 lines of logs).

## Snippet Composition Rules

Snippets are token-efficient building blocks.

When composing multi-step workflows:

```
Step 1: Load {snippet-1}
  - Max input: 400 tokens
  - Run independently
  - Save output to qa-memory

Step 2: Load {snippet-2}
  - Input: Step 1 output + qa-context reference
  - Max input: 400 tokens
  - Run independently

Step 3: Load {hook}
  - Input: Step 1 + Step 2 outputs
  - Max input: 600 tokens
  - Produce final decision
```

## When to Stop Saving to qa-memory

If a finding is:
- Temporary debug info → discard
- Already in qa-context → don't duplicate
- Transient failure → archive only root cause
- Standard pattern → reference existing pattern note

## Token Reporting

Every hook/snippet output should note:
```
Input tokens used: 450 / 800
Output tokens used: 250 / 400
Context efficiency: HIGH / MEDIUM / LOW
(HIGH = reused prior analysis, LOW = full rebuild needed)
```
