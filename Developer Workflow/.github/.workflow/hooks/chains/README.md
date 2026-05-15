# Hook Chains: Automated Multi-Step Workflows

Hook chains are sequential execution plans that guide you through complete workflows without manual prompting between steps.

## Chain: new-feature

Complete workflow for implementing a new feature from requirement to PR.

```
START: New Feature
  ↓
pre-task.context-load
  ↓
Run Requirement Agent
  ↓
post-impact.memory-snapshot
  ↓
Run Impact Agent (use context from pre-task)
  ↓
post-requirement.auto-create-spec
  ↓
post-spec.approval-gate ⛔ (user must approve)
  ↓
pre-implementation.scope-check
  ↓
Run Implementation Agent
  ↓
post-implementation.context-update
  ↓
post-implementation.test-generation
  ↓
Run Test Agent (use generated test output)
  ↓
pre-review.spec-check
  ↓
quality.review-diff
  ↓
post-review.findings-archive
  ↓
pre-pr.gate-readiness ⛔ (validate all checks)
  ↓
Create PR
  ↓
post-pr.changelog-update
  ↓
END: Feature complete
```

**Token Budget**: ~4000 tokens total spread across all steps.

## Chain: bug-fix

Workflow for debugging, fixing, and validating a bug.

```
START: Bug Report (logs + failure)
  ↓
Run Debug Agent
  ↓
post-debug.memory-archive (save root cause)
  ↓
Generate fix patch
  ↓
post-implementation.test-generation (create regression test)
  ↓
Run tests
  ↓
pre-review.spec-check (if bug is tracked in spec)
  ↓
quality.review-diff
  ↓
Create PR or hotfix
  ↓
post-pr.changelog-update (mark as "Fixed")
  ↓
END: Bug resolved
```

**Token Budget**: ~2000 tokens.

## Chain: refactor

Workflow for safe refactoring with regression protection.

```
START: Refactor Plan
  ↓
Run Impact Agent (find all affected code)
  ↓
post-impact.memory-snapshot (save scope)
  ↓
Run Design Agent (plan refactor approach)
  ↓
Run Implementation Agent (refactor code)
  ↓
Run Test Agent (generate regression tests for changed behavior)
  ↓
Verify no behavior change:
  quality.review-diff (compare with original behavior)
  ↓
post-implementation.context-update (save decision)
  ↓
context.update-patterns (if new pattern discovered)
  ↓
Create PR
  ↓
post-pr.changelog-update
  ↓
END: Refactor merged
```

**Token Budget**: ~3000 tokens.

## Chain: api-change

Workflow for breaking API changes with migration planning.

```
START: API Change Requirement
  ↓
Run Spec Agent (document new contract + migration)
  ↓
post-spec.approval-gate ⛔ (mandatory approval for breaking change)
  ↓
Run Implementation Agent
  ↓
quality.detect-breaking-changes ⛔ (validate migration plan)
  ↓
post-implementation.test-generation (backward compat tests)
  ↓
Run tests
  ↓
quality.review-diff
  ↓
post-review.findings-archive
  ↓
Create PR (link migration plan in description)
  ↓
post-pr.changelog-update (mark as "Changed" with migration note)
  ↓
END: API migration planned
```

**Token Budget**: ~3500 tokens.

## Execution Rules

1. Start at the first hook.
2. Follow arrows in sequence.
3. Stop at gates (⛔) and wait for user approval.
4. Reference memory notes instead of re-scanning.
5. Never skip a step.
