# Hook Library Catalog

## Hook Types

### 1. Workflow Hooks (Pre/Post Task)
- `pre-task.context-load`: Load spec, decisions, patterns before starting
- `post-impact.memory-snapshot`: Save impact analysis to memory
- `post-requirement.auto-create-spec`: Auto-draft spec from requirement + impact
- `post-spec.approval-gate`: Gate implementation on spec approval
- `pre-implementation.scope-check`: Verify implementation scope matches spec
- `post-implementation.context-update`: Update memory with implementation decision
- `post-implementation.test-generation`: Generate tests from changed code
- `post-refactor.regression-check`: Verify refactoring doesn't break behavior
- `post-debug.memory-archive`: Save bug root cause to memory
- `pre-review.spec-check`: Verify diff against spec before review
- `post-review.findings-archive`: Archive review findings to decision log
- `pre-pr.gate-readiness`: Validate PR readiness (tests, docs, context)
- `post-pr.changelog-update`: Update changelog automatically

### 2. Context Hooks (Maintenance)
- `context.update-modules`: Refresh modules.md based on changed files
- `context.update-workflows`: Update workflow descriptions
- `context.update-dependencies`: Sync dependency changes
- `context.update-patterns`: Extract and save new patterns discovered
- `context.update-decisions`: Log architectural decisions

### 3. Spec Hooks (Spec-DD)
- `spec.create`: Generate spec from requirement
- `spec.refine`: Improve existing spec based on feedback
- `spec.validate`: Check spec consistency and completeness
- `spec.link-to-decision`: Link spec to decision log

### 4. Quality Hooks (Testing & Review)
- `quality.generate-tests`: Create tests from spec scenarios
- `quality.review-diff`: Perform risk-focused code review
- `quality.detect-breaking-changes`: Identify API breaks
- `quality.detect-missing-docs`: Flag undocumented changes

### 5. Chain Hooks (Multi-Step)
- `chain.new-feature`: requirement → impact → spec → design → implementation → tests
- `chain.bug-fix`: logs → debug → fix → regression-test → review
- `chain.refactor`: impact → design → implementation → regression-tests → review
- `chain.api-change`: spec → impact → breaking-change-check → migration-plan

## Hook Usage Matrix

| Situation | Primary Hook | Secondary | Output |
|---|---|---|---|
| New feature starts | `pre-task.context-load` | - | Minimal context bundle |
| Feature impact identified | `post-impact.memory-snapshot` | - | Saved decision note |
| Requirement + impact complete | `post-requirement.auto-create-spec` | `post-spec.approval-gate` | Draft spec + approval gate |
| Spec created | `post-spec.approval-gate` | - | Approval block |
| Ready to code | `pre-implementation.scope-check` | - | Scope validation |
| Code complete | `post-implementation.context-update` | `post-implementation.test-generation` | Updated memory + tests |
| Bug found | `post-debug.memory-archive` | - | Bug note saved |
| Before PR | `pre-review.spec-check` | `quality.review-diff` | Findings + readiness |
| PR merged | `post-pr.changelog-update` | - | Updated changelog |

## Storage Structure

```
.copilot/hooks/
  pre-task/
    context-load.prompt.md
  implementation/
    pre-implementation.scope-check.prompt.md
    post-implementation.context-update.prompt.md
    post-implementation.test-generation.prompt.md
  debug/
    post-debug.memory-archive.prompt.md
  review/
    pre-review.spec-check.prompt.md
    post-review.findings-archive.prompt.md
  context/
    update-modules.prompt.md
    update-patterns.prompt.md
    update-decisions.prompt.md
  spec/
    post-requirement.auto-create-spec.prompt.md
    create.prompt.md
    validate.prompt.md
  quality/
    generate-tests.prompt.md
    review-diff.prompt.md
    detect-breaking-changes.prompt.md
  chains/
    new-feature.chain.md
    bug-fix.chain.md
    refactor.chain.md
  pr/
    pre-pr.gate-readiness.prompt.md
    post-pr.changelog-update.prompt.md
```

## Token Budgets (Strict)

| Hook Type | Max Input Tokens | Max Output Tokens |
|---|---|---|
| pre-task | 300 | 200 |
| post-impact | 400 | 150 |
| pre-implementation | 500 | 100 |
| post-implementation.context | 600 | 200 |
| post-implementation.tests | 700 | 500 |
| post-debug | 500 | 200 |
| pre-review | 800 | 300 |
| quality.review-diff | 900 | 400 |
| pre-pr.gate | 700 | 150 |
