# Spec References and Context Boundaries

## Core Principle

**Always reference specs by path. Never repeat raw requirements in prompts.**

## Why This Matters

- Reduces prompt bloat and token waste
- Forces consistent spec updates (single source of truth)
- Enables faster context loading for agents
- Reduces misalignment between requirement and test

## Spec Reference Format

Always use this pattern:

```
Spec: specs/<feature-name>.md

When needed, call out specific section:
Spec: specs/<feature-name>.md#Business Logic
Spec: specs/<feature-name>.md#Edge Cases
```

## Never Do This

❌ Bad:
```
The payment retry API should:
- Retry failed payments up to 3 times
- Exponential backoff with jitter
- ...
```

✅ Good:
```
Spec: specs/payment-retry-api.md

(Spec contains all details, no repetition in prompt)
```

## Affected Files and Modules Reference

When calling out what changed, use this format:

```
Changed modules:
- src/payment/retry-strategy.ts
- src/payment/exponential-backoff.ts
- src/database/payment-event-log.ts

Changed APIs:
- POST /api/v1/payments/{id}/retry
- GET /api/v1/payments/{id}/retry-history
```

## QA Context References

Reference stable QA artifacts instead of rebuilding them:

```
Test strategy: qa-context/test-strategy.md
Security model: qa-context/security-model.md
Regression map: qa-context/regression-map.md
API matrix: qa-context/api-test-matrix.md
Flaky test list: qa-context/flaky-tests.md
Environment matrix: qa-context/environments.md
```

## Memory References

Link findings to existing memory to avoid re-analysis:

```
Related bug: qa-memory/bugs/[bug-id].md
Related security finding: qa-memory/security/[finding-id].md
Related healing analysis: qa-memory/healing/[analysis-id].md
Related regression pattern: qa-memory/regression/[pattern-id].md
```

## Context Boundaries by Phase

### Phase 1: Requirement QA
```
Input context: Requirement note only
Reference: spec template (not spec itself)
Max tokens: 500
Output: summary + ambiguities
```

### Phase 2: QA Impact
```
Input context: Requirement summary + entry points
References: specs/<feature>.md (summary section only)
Max tokens: 700
Output: affected modules + risk hotspots
```

### Phase 3: Spec QA Analysis
```
Input context: Full spec + requirement summary
References: specs/<feature>.md (full)
Max tokens: 900
Output: QA strategy + test matrix
```

### Phase 4: Risk & Threat Analysis
```
Input context: Spec + impact map from Phase 2
References: specs/<feature>.md, qa-context/security-model.md
Max tokens: 900
Output: risk register + threat model updates
```

### Phase 5-10: Execution & Validation
```
Input context: Spec + changed modules + test results
References: specs/<feature>.md, qa-context/*, qa-memory/*
Max tokens: 800-1000 (per phase)
Output: test results + security verdict + release decision
```

## Context Loading Rule

Before ANY prompt:

1. ✅ Load spec by path (reference, not full text)
2. ✅ Load only changed modules (diffs, not full files)
3. ✅ Load relevant qa-context file (strategy artifact)
4. ✅ Load relevant qa-memory note (if prior analysis exists)
5. ❌ Do NOT load entire repo
6. ❌ Do NOT load entire test suite
7. ❌ Do NOT repeat requirement text

## Diff-First Strategy

When changed code is needed:

```
Changed diff:
```diff
- old code
+ new code
```

NOT full file dump.

When full file is needed:
- Justify why (e.g., "need to understand entire retry loop logic")
- Limit to one file
- Mark sections with comments: `// --- CONTEXT START ---`

## Phase Transitions

Each phase transition should:

1. Update relevant qa-context file (if new info discovered)
2. Add findings to qa-memory (if significant)
3. Pass next phase ONLY approved spec/impact/risk/etc
4. Include gate approval evidence

## Tool-Agnostic References

Never hardcode IDE-specific paths:

❌ Bad: `.vscode/chat-history.json`
✅ Good: `qa-memory/` (tool-independent path)

All references use workspace-relative paths with `/` forward slashes.
