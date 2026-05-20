# Phase Constraints & Resource Budgets

## Overview

Each workflow phase has specific constraints:
- **Token budget** (max LLM tokens)
- **Time budget** (max wall-clock time)
- **Context window** (max input tokens)
- **Output constraints** (max output size)
- **Retry limits** (max retry attempts)
- **Tool access** (which tools available)

## Token Budget System

### Philosophy
- **Minimal but sufficient**: Each phase gets enough tokens to complete, no more
- **Cumulative tracking**: Total workflow tokens tracked against global limit
- **Reuse enforcement**: Memory/specs preferred over re-scanning code

### Phase Token Budgets

| Phase | Tokens | Justification | Flexible? |
|---|---|---|---|
| 1. Requirement | 500 | Parse requirement + list ambiguities | Yes +100 (questions) |
| 2. Impact | 700 | Scan codebase + trace dependencies | Yes +200 (complex) |
| 3. Spec | 900 | Draft detailed spec with test cases | Yes +200 (complex) |
| 4. Design | 900 | Architecture + migration (optional) | Yes +300 (novel) |
| 5. Implementation | 1200 | Generate minimal code | Yes +500 (complex) |
| 6. Testing | 900 | Generate test cases | Yes +200 (edge cases) |
| 7. Debug | 900 | Diagnose + fix (loop as needed) | Yes +500 (complex) |
| 8. Refactor | 1000 | Suggest refactoring (optional) | No (skip if over) |
| 9. Review | 1500 | Code review + readiness checks | No (hard limit) |
| 10. Documentation | 700 | Update docs/changelog | No (hard limit) |
| **TOTAL** | **~9,000** | **Full workflow** | - |

### Token Usage Rules

#### Rule 1: Count Incoming Context
Every prompt must report:
```
Context tokens: [X] (incoming)
Max available: [Y]
Tokens for response: [Y - X]
```

Example:
```
Context tokens: 350 (spec + file snippets)
Max available: 500
Tokens for response: 150
```

#### Rule 2: Reuse Over Rescanning
```
❌ BAD: "Here's the entire payment.py file (400 tokens)..."
✓ GOOD: "See spec/payment-processing.md line 42-58 for API"
```

**Savings**: ~300 tokens per reference

#### Rule 3: Selective Memory Loading
```
❌ BAD: Load all memory notes (5+ KB)
✓ GOOD: Load 2-3 most recent + relevant notes (~500 tokens)
```

**Savings**: ~1000+ tokens per session

#### Rule 4: Diff-First for Code Review
```
❌ BAD: "Here's the entire updated payment.py (600 tokens)..."
✓ GOOD: "Diff: [lines 42-58 changed from X to Y]" (100 tokens)
```

**Savings**: ~500 tokens per review

#### Rule 5: Stop at Constraint
If phase hits token limit mid-execution:
```
@500 tokens used / 500 available
→ STOP generation
→ Output what's complete so far
→ User can continue in next session (context preserved in memory)
```

#### Rule 6: Overflow Budget Fallback
If phase needs more tokens:
```
Current phase: 800 tokens used / 900 available
Needed for completion: +150 tokens
Available overflow: +200 (from contingency)

→ Use overflow
→ Log to memory: "Phase 5 overflowed by 50 tokens"
→ Subsequent phases reduce by 50 tokens each
```

### Monitoring Tokens

Every agent outputs:
```
=== Phase Report ===
Tokens used: 650 / 900 available
Tokens saved (reuse): 400
Tokens spent (new context): 250
Efficiency: 65% (goal: >60%)
```

## Time Budget System

### Recommended Durations

| Phase | Solo Dev | Team | Hotfix | Notes |
|---|---|---|---|---|
| 1. Requirement | 10 min | 15 min | 5 min | Faster if spec exists |
| 2. Impact | 10 min | 15 min | 5 min | Depends on codebase size |
| 3. Spec | 30 min | 60 min | Skip | Spec approval = 2-24h wait |
| 4. Design | 15 min | 30 min | Skip | Optional if spec detailed |
| 5. Implementation | 45 min | 90 min | 15 min | Depends on feature size |
| 6. Testing | 20 min | 30 min | 10 min | Test coverage needed |
| 7. Debug | Varies | Varies | Varies | Loop as needed |
| 8. Refactor | 15 min | 30 min | Skip | Optional |
| 9. Review | 20 min | 45 min | 10 min | Includes readiness checks |
| 10. Documentation | 20 min | 30 min | 10 min | Minimal for hotfix |
| **TOTAL** | **~3-4h** | **~4-5h** | **~45 min** | - |

### Clock-Out Rules
Phase must complete within time budget or:
1. **Soft timeout** (90% of budget) → Warn user
2. **Hard timeout** (100% of budget) → Save progress to memory → Continue in next session

Example:
```
Phase 5 Implementation
Time limit: 45 minutes
Current: 42 minutes elapsed
Status: 85% complete (code generated, needs review)

→ WARNING: Approaching time limit
→ Save partial code to memory
→ User can continue in next session
```

## Context Window Constraints

### Input Context Limits

| Phase | Max Input | Typical | Rationale |
|---|---|---|---|
| Requirement | 2 KB | 0.5 KB | Raw requirement (concise) |
| Impact | 10 KB | 3 KB | Codebase map + entry points |
| Spec | 5 KB | 2 KB | Requirement + impact list |
| Design | 8 KB | 4 KB | Spec + codebase structure |
| Implementation | 15 KB | 8 KB | Spec + target files (diffs) |
| Testing | 12 KB | 6 KB | Spec + implementation diffs |
| Review | 20 KB | 10 KB | Spec + full diff + tests |
| Documentation | 10 KB | 5 KB | Changes + spec summary |

### Output Size Limits

| Phase | Max Output | Typical | Notes |
|---|---|---|---|
| Requirement | 1 KB | 0.5 KB | Structured summary |
| Impact | 2 KB | 1 KB | File list + reasons |
| Spec | 5 KB | 3 KB | Detailed spec (40-50 lines) |
| Design | 4 KB | 2 KB | Architecture notes |
| Implementation | Variable* | 3-10 KB | Diff size depends on feature |
| Testing | Variable* | 2-5 KB | Test file size |
| Review | 3 KB | 1.5 KB | Structured findings |
| Documentation | 2 KB | 1 KB | Docs update + changelog |

*Implementation/Testing outputs can exceed limits if feature large; split into multiple phases if needed.

## Retry Limits

### Phase 1-4 (Analysis)
- Max retries: 3
- Timeout between: 30 seconds
- On failure: Escalate to human

### Phase 5 (Implementation)
- Max retries: 5 (code generation prone to error)
- Timeout between: 1 minute
- On failure: Escalate to Debug phase or human

### Phase 6 (Testing)
- Max retries: 3
- Timeout between: 1 minute
- On failure: Escalate to Debug phase

### Phase 7 (Debug)
- Max retries: 5 (can loop indefinitely)
- Timeout between: 2 minutes
- On failure after 5 retries: Escalate to human

### Phase 9 (Review)
- Max retries: 2 (automated checks)
- Human review is not retried (human decides)
- On failure: Assign to reviewer

### Retry Escalation
```
Retry 1 → Try same approach
Retry 2 → Try different agent parameters
Retry 3 → Try different tool
Retry 4 → Escalate + log to memory
Retry 5 → Stop + require human intervention
```

## Tool Access Constraints

### Tools Available Per Phase

| Tool | Req | Impact | Spec | Impl | Test | Review |
|---|---|---|---|---|---|---|
| Read files | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Git diff | - | - | - | ✓ | ✓ | ✓ |
| Run tests | - | - | - | - | ✓ | ✓ |
| Search codebase | ✓ | ✓ | - | ✓ | - | ✓ |
| Generate files | - | - | ✓ | ✓ | ✓ | - |
| Call LLM | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Execute code | - | - | - | - | ✓ | - |
| Merge branches | - | - | - | - | - | ✓ |
| Create PRs | - | - | - | - | - | ✓ |

### Permission Model
- Requirement/Impact: Read-only (no code changes)
- Spec: Can create spec file
- Implementation: Can modify target files only
- Testing: Can create test files
- Review: Can merge PRs
- Documentation: Can update docs

## Resource Contention

### Parallel Phase Execution
When multiple workflows run simultaneously:

1. **Token priority** (allocate tokens fairly):
   - New features: 40%
   - Bug fixes: 40%
   - Refactoring: 20%

2. **Time sharing**:
   - Long phases (Impl, Testing) → Round-robin scheduling
   - Short phases (Req, Impact) → FIFO (high priority)

3. **Codebase access**:
   - Read: Unlimited
   - Write: Exclusive (prevent conflicts)
   - Merge: Exclusive + coordinated

### Contention Detection
```
Conflict detected: Phase 5 (Impl) modifying src/payment/models.py
                  Phase 7 (Debug) also modifying same file
→ Queue Phase 7 Debug
→ Notify Phase 5 that Phase 7 waiting
→ Once Phase 5 complete → Phase 7 can proceed
```

## Constraint Violations & Recovery

### Token Budget Exceeded
```
Phase 5 Implementation
Budget: 1200 tokens
Used: 1450 tokens (EXCEEDED by 250)

Recovery:
1. Log overflow to memory
2. Reduce Phase 9 budget by 250 (1500 → 1250)
3. Check if total workflow exceeds 9000 (global limit)
   └─ If yes: Escalate + plan split workflow
```

### Time Budget Exceeded
```
Phase 5 Implementation
Budget: 45 minutes
Used: 50 minutes (EXCEEDED by 5)

Recovery:
1. Check: Is code generation complete?
   └─ YES: Continue to Phase 6 (acceptable overage)
   └─ NO: Save partial code + continue next session
2. Log overage to memory for trend analysis
```

### Output Size Exceeded
```
Phase 5 Implementation generated 12 KB diff
Max output: Not enforced (feature large)

Recovery:
1. Accept large output
2. Consider splitting into multiple PRs:
   - PR1: Models + migrations
   - PR2: API layer
   - PR3: Tests
3. Create separate workflows for each PR
4. Reference shared spec in all PRs
```

## Constraint Tuning

### Monthly Review
Analyze constraint effectiveness:
- Are phases consistently hitting limits?
- Are time estimates accurate?
- Are token budgets sufficient?

### Adjustment Process
If constraint is too tight:
```
Observation: Phase 5 consistently overflows by 200 tokens
Decision: Increase budget 1200 → 1400
Impact: Total workflow increases ~200 tokens
Tradeoff: Faster execution vs. slightly higher cost

→ Approve adjustment
→ Update all documentation
```

## Emergency Constraints

### Hotfix Mode
Apply stricter constraints:
- Token budget: -20% (faster execution)
- Time budget: -50% (emergency only)
- Skip: Design (Phase 4), Refactoring (Phase 8)
- Approve: VP/CTO required if constraints breached

### Maintenance Window
Apply relaxed constraints:
- Token budget: +50% (thorough testing)
- Time budget: +30% (comprehensive review)
- Add: Performance profiling + optimization

## Constraint Documentation

Every workflow must report:
```
=== Workflow Constraints Report ===
Tokens: 1200/1200 used (100%)
Time: 45m/45m used (100%)
Context input: 8KB / 15KB limit
Output: 5.2KB / Variable limit
Retries: 0 / 5 allowed
Tools used: [ReadFile, GitDiff, RunTests, CreateFiles]
Constraints exceeded: None
Efficiency score: 92/100

Recommendations for next time:
- Load fewer files initially (context was tight)
- Reuse spec references better
- Consider splitting into 2 PRs if feature >10KB
```
