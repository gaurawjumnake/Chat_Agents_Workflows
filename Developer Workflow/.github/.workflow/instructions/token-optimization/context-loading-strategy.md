# Token Optimization Strategy

## Overview

Token optimization is critical for cost efficiency and speed. This workflow achieves **50-60% token reduction** through 5 core strategies:

1. **Memory persistence** - Store once, reference thereafter
2. **Spec discipline** - Single source of truth (never restate)
3. **Diff-first** - Review changes, not full code
4. **Selective loading** - Load only relevant context
5. **Minimal prompts** - Small focused requests, not massive context dumps

## Strategy 1: Memory Persistence

### Philosophy
Every decision is stored once and referenced infinitely.

**Without memory** (❌ Wasteful):
```
Session 1: State problem (500 tokens) → Solution (1000 tokens)
Session 2: Re-state problem (500 tokens) → Solution (1000 tokens) [DUPLICATE]
Session 3: Re-state problem (500 tokens) → Solution (1000 tokens) [DUPLICATE]

Total: 4500 tokens (200% waste)
```

**With memory** (✓ Efficient):
```
Session 1: State problem (500 tokens) → Solution (1000 tokens)
         → Save decision to memory (100 tokens)
Session 2: Load memory (50 tokens) → Continue solution (200 tokens)
Session 3: Load memory (50 tokens) → Continue solution (200 tokens)

Total: 2100 tokens (53% reduction)
```

### Memory Persistence Rules

1. **After Spec Approval** (Phase 3)
   - Save: Approved spec + key decisions
   - Ref: Always cite spec path instead of restating requirement

2. **After Implementation** (Phase 5)
   - Save: Architecture decisions + trade-offs
   - Ref: Load decision notes for future similar work

3. **After Debugging** (Phase 7)
   - Save: Root cause + fix strategy
   - Ref: Use pattern to prevent similar bugs

4. **After Review** (Phase 9)
   - Save: Code review findings + patterns
   - Ref: Apply learnings to future reviews

### Memory Loading in Prompts

**Bad** (includes full memory):
```
Implement feature.

Here's memory from 5 previous sessions:
  Decision 1 (500 tokens): [full text]
  Decision 2 (400 tokens): [full text]
  Decision 3 (300 tokens): [full text]

[Feature implementation request]
```

**Good** (minimal memory reference):
```
Implement feature.

Context references:
- Decision: memory/decisions/2026-05-15-async-pattern.md
- Pattern: memory/patterns/retry-strategy.md
- Bug: memory/incidents/2026-05-10-timeout-issue.md

[Feature implementation request]
```

**Savings**: ~1200 tokens (memory not included in prompt)

## Strategy 2: Spec Discipline

### Philosophy
Spec is the single source of truth. Never restate requirement after approval.

### Pattern: Ref > Restate

**Bad** (repeats requirement):
```
The feature must:
1. Take amount parameter
2. Validate amount > 0
3. Process payment with retry logic
4. Return receipt with transaction ID
5. Handle 5 error cases

Please implement this...
```
(This is ~200 tokens of repetition if requirement already in spec)

**Good** (references spec):
```
Implement feature per specs/payment-processing.md

Target files:
- src/payment/processor.py (lines 42-58)
- src/payment/models.py

Please implement...
```
(This is ~50 tokens; saves ~150 tokens)

### Rule: Spec Path Always
Every implementation/test/review request MUST include:
```
Spec: specs/[feature-name].md
```

Agents will:
1. Load spec (if not already loaded)
2. Extract requirements
3. Implement against spec

No need to re-state requirements in prompt.

### Savings Calculation
- Average requirement restatement: 200 tokens
- Specs saved per workflow: 5-7 (Req, Impl, Test, Review, Doc)
- Total savings: 1000-1400 tokens per workflow

## Strategy 3: Diff-First Review

### Philosophy
Review changes, not full files.

### Pattern: Diff > Full Code

**Bad** (full code review):
```
Here's the updated payment.py file:

```python
[500 lines of code]
...
```

Please review this file...
```
(This uses ~400 tokens for full file)

**Good** (diff-first review):
```
Changes to payment.py:

Lines 42-58 (changed):
- Before: amount = int(amount)
+ After: if amount <= 0: raise ValueError()
         amount = int(amount)

Lines 80-90 (added):
+ def validate_payment_async():
+   [new function]

Please review these changes...
```
(This uses ~80 tokens; saves ~320 tokens)

### Diff-First Rules

1. **Always use git diff** (not full files)
   ```
   git diff src/payment/processor.py
   → Output: Only changed lines (with context)
   ```

2. **Use line ranges for references**
   ```
   "Line 42-58 handles validation"
   NOT "Here's the entire Validator class (200 lines)"
   ```

3. **Mention only changed functions**
   ```
   "Function validate_payment() changed to include retry logic"
   NOT "Here's all 50 functions in this module"
   ```

4. **Diff output limits**
   - Small change (<100 lines): Include full diff
   - Medium change (100-500 lines): Summary + key diff sections
   - Large change (>500 lines): Summary + file-by-file diff

### Savings Calculation
- Typical file size: 500 lines = 300 tokens
- Typical diff size: 50 lines = 50 tokens
- Savings per review: ~250 tokens

## Strategy 4: Selective Context Loading

### Philosophy
Load only what's needed; let agent load the rest.

### Pattern: Minimal Input Context

**Bad** (loads everything):
```
Here's the context for implementing payment feature:

1. Full payment.py file (400 tokens)
2. Full models.py file (300 tokens)
3. Full database schema (200 tokens)
4. Full API documentation (300 tokens)
5. Full payment-related memory (500 tokens)
6. Full spec (500 tokens)

[Feature implementation request]

Total context: 2200 tokens
```

**Good** (loads selectively):
```
Spec: specs/payment-processing.md
Target files: src/payment/processor.py, src/payment/models.py
Reference pattern: memory/patterns/async-processing.md

Please implement...
```
(Context references only; agent loads if needed)

**Savings**: ~2000 tokens (context loaded on-demand)

### Selective Loading Rules

1. **Reference, don't embed**
   ```
   ✓ "See specs/payment-processing.md for API"
   ✗ "[Include full 500-line spec file]"
   ```

2. **Load only affected code**
   ```
   ✓ "Modify lines 42-58 in src/payment/processor.py"
   ✗ "[Include entire 500-line file]"
   ```

3. **Memory by keyword search**
   ```
   ✓ "Load memory: incidents/keyword=timeout"
   ✗ "[Include all 50 incident files]"
   ```

4. **Architecture by link**
   ```
   ✓ "See docs/payment/architecture.md for system design"
   ✗ "[Include 3000-token architecture description inline]"
   ```

### Context Loading Priorities

When loading context, prioritize by relevance:
```
1. Spec (highest priority) - Source of truth
2. Recent related memory (high) - Most relevant
3. Target files (high) - Code to modify
4. Related tests (medium) - Behavior reference
5. Architecture docs (low) - Context only
6. Historical decisions (low) - Reference only
```

### Savings Calculation
- Typical full context dump: 2000-3000 tokens
- With selective loading: 300-500 tokens
- Savings per session: ~2000 tokens

## Strategy 5: Minimal Prompts

### Philosophy
Small focused prompts are more efficient than massive context dumps.

### Pattern: Focused > Comprehensive

**Bad** (massive prompt):
```
Here's the full context for payment processing:

[2000 tokens of background]

The requirement is:
[500 tokens of requirement]

Here's the spec:
[1000 tokens of spec]

Now please:
- Generate tests
- Consider edge cases
- Handle errors
- Optimize performance
- Document behavior
- [List 20 more tasks]

[Feature implementation request]

Total prompt: 3500+ tokens
```

**Good** (focused prompts):
```
PROMPT 1 (Phase 5: Implementation):
Spec: specs/payment-processing.md
Target: src/payment/processor.py (lines 42-58)
Task: Generate code changes only

[200 tokens total]

---

PROMPT 2 (Phase 6: Testing):
Spec: specs/payment-processing.md (test scenarios)
Code: [recent implementation diff]
Task: Generate test cases only

[250 tokens total]

---

PROMPT 3 (Phase 9: Review):
Spec: specs/payment-processing.md
Code: [implementation diff + tests]
Task: Review for errors and risks

[300 tokens total]
```

**Savings**: ~2500 tokens (broken into 3 focused prompts)

### Focused Prompt Rules

1. **One task per prompt**
   ```
   ✓ "Generate code for Phase 5"
   ✗ "Generate code, tests, and documentation"
   ```

2. **Specific output requested**
   ```
   ✓ "Output: Python code only (no explanation)"
   ✗ "Generate code and explain your thinking"
   ```

3. **Bounded scope**
   ```
   ✓ "Modify lines 42-58 in processor.py only"
   ✗ "Refactor payment module (might touch 10 files)"
   ```

4. **Limited options**
   ```
   ✓ "Generate tests for these 5 scenarios: [list]"
   ✗ "Generate comprehensive test coverage"
   ```

## Optimization Checklist

Before every prompt, verify:

- [ ] **Memory persistence**: Is decision already in memory? Reference it instead of restating
- [ ] **Spec discipline**: Is spec path cited? Avoid restating requirement
- [ ] **Diff-first**: Using diffs instead of full files? (if code review)
- [ ] **Selective loading**: Context references only? (no massive inline context)
- [ ] **Minimal prompt**: One task? Bounded scope? Specific output?

## Token Audit Trail

Every agent logs:
```
=== Token Audit ===
Phase: [Phase name]
Incoming context: [X] tokens
- Spec reference: [Y] tokens (if loaded)
- Memory reference: [Z] tokens (if loaded)
- Code diff: [W] tokens (if loaded)
Output: [V] tokens
Total: [X + V] tokens

Efficiency score: [% efficiency vs budget]
```

Example:
```
=== Token Audit ===
Phase: Implementation (Phase 5)
Incoming context: 250 tokens
  - Spec reference: 50 (not loaded, just path)
  - Code context: 200 (target file snippet)
Output: 800 tokens (implementation code)
Total: 1050 tokens / 1200 budget

Efficiency: 87% (Goal: >80%)
Techniques used: [Diff-first, Selective loading, Spec discipline]
```

## Token Budget Allocation (Full Workflow)

```
Total available: 9000 tokens

Phase 1 (Requirement): 500 tokens
  - Techniques: Minimal prompt
  - Savings: ~100 tokens

Phase 2 (Impact): 700 tokens
  - Techniques: Selective loading
  - Savings: ~200 tokens

Phase 3 (Spec): 900 tokens
  - Techniques: Memory persistence (save decision)
  - Savings: ~100 tokens

Phase 4 (Design): 900 tokens optional
  - Techniques: Spec discipline (reference only)
  - Savings: ~200 tokens

Phase 5 (Implementation): 1200 tokens
  - Techniques: Spec discipline + Selective loading
  - Savings: ~400 tokens

Phase 6 (Testing): 900 tokens
  - Techniques: Spec discipline (test scenarios)
  - Savings: ~200 tokens

Phase 7 (Debug): 900 tokens +flexible
  - Techniques: Memory persistence (load bug patterns)
  - Savings: ~300 tokens

Phase 8 (Refactor): 1000 tokens optional
  - Techniques: Minimal prompts
  - Savings: ~100 tokens

Phase 9 (Review): 1500 tokens
  - Techniques: Diff-first + Memory persistence
  - Savings: ~600 tokens

Phase 10 (Documentation): 700 tokens
  - Techniques: Spec discipline
  - Savings: ~100 tokens

TOTAL SAVINGS: ~2200 tokens (24% reduction)
ADDITIONAL REUSE: ~1200 tokens (13% more savings from cross-session memory)
TOTAL: ~50-60% reduction vs non-optimized workflow
```

## Session-to-Session Optimization

### Session 1 (Full workflow)
- Tokens: 9000 (new feature)
- Memory saved: Spec + decisions

### Session 2 (Continue after break)
- Load memory: 200 tokens (spec + decisions loaded)
- Continue from Phase 9: 1500 tokens (review)
- Total: 1700 tokens (not full 9000)

### Session 3 (Similar feature)
- Load memory: 300 tokens (patterns + decisions)
- Load spec: 50 tokens (reference)
- New implementation: 1200 tokens
- Total: 1550 tokens (60% reduction vs Session 1)

## Real-World Example

**Feature**: "Add idempotency to POST /payments API"

### Without Optimization (❌)
```
Session 1:
  - Restate requirement: 200 tokens
  - Restate payment system: 300 tokens
  - Full implementation: 1000 tokens
  - Full tests: 800 tokens
  - Full review: 600 tokens
  Total: 2900 tokens

Session 2 (Bug found):
  - Restate requirement: 200 tokens
  - Restate payment system: 300 tokens
  - Debug fix: 700 tokens
  Total: 1200 tokens

TOTAL: 4100 tokens
```

### With Optimization (✓)
```
Session 1:
  - Reference spec: 50 tokens (path only)
  - Selective implementation: 800 tokens
  - Focused tests: 600 tokens
  - Diff-first review: 300 tokens
  - Save to memory: 100 tokens
  Total: 1850 tokens

Session 2 (Bug found):
  - Load memory: 100 tokens (reference)
  - Load spec: 50 tokens (path only)
  - Debug fix: 400 tokens
  Total: 550 tokens

TOTAL: 2400 tokens (41% reduction)
COST SAVING: 1700 tokens (~40 cents per workflow)
```

## Ongoing Optimization

### Weekly Review
- Track average tokens per workflow
- Identify phases with high token usage
- Suggest optimization techniques

### Monthly Tuning
- Adjust phase budgets based on actual usage
- Update techniques based on effectiveness
- Share learnings with team

### Continuous Improvement
- New optimization technique discovered? → Add to this guide
- Technique not working? → Remove or modify
- Benchmark against industry standards
