# Hook Dependency Graph & Trigger Rules

This diagram shows how hooks connect and which are dependent on others.

## Dependency Graph (Text Representation)

```
Task Start
  │
  └─→ pre-task.context-load
       │ (Load spec, decisions, patterns)
       │
       ├─→ Requirement Agent
       │    │
       │    └─→ post-impact.memory-snapshot
       │         │ (Save impact decision)
       │         │
       │         └─→ Impact Agent
       │              │
       │              ├─→ Spec Agent
       │              │    │
       │              │    └─→ post-spec.approval-gate ⛔
       │              │         │ (GATE: Must approve)
       │              │         │
       │              │         └─→ pre-implementation.scope-check
       │              │              │
       │              │              └─→ Implementation Agent
       │              │                   │
       │              │                   ├─→ post-implementation.context-update
       │              │                   │    │ (Save impl decision)
       │              │                   │
       │              │                   └─→ post-implementation.test-generation
       │              │                        │ (Generate tests)
       │              │                        │
       │              │                        └─→ Test Agent
       │              │                             │
       │              │                             ├─→ pre-review.spec-check
       │              │                             │    │
       │              │                             │    └─→ quality.review-diff
       │              │                             │         │
       │              │                             │         ├─→ quality.detect-breaking-changes
       │              │                             │         │
       │              │                             │         └─→ post-review.findings-archive
       │              │                             │              │ (Save findings)
       │              │                             │
       │              │                             └─→ pre-pr.gate-readiness ⛔
       │              │                                  │ (GATE: All checks passed?)
       │              │                                  │
       │              │                                  └─→ Create PR
       │              │                                       │
       │              │                                       └─→ post-pr.changelog-update
       │              │                                            │ (Update changelog)
       │              │
       │              └─→ context.update-patterns
       │                   (Save discovered patterns)
       │
       └─→ post-debug.memory-archive
            (When bug is fixed)
```

## Hook Trigger Conditions

### Auto-Triggered (No Input Needed)
None. All hooks require explicit invocation via chat.

### Requires User Approval (Gates)
- `post-spec.approval-gate`: Must approve before implementation
- `pre-pr.gate-readiness`: Must verify all checks before PR

### Depends on Previous Outputs
- `pre-implementation.scope-check`: Requires approved spec
- `pre-review.spec-check`: Requires changed files/diff
- `quality.review-diff`: Requires passing pre-review.spec-check
- `detect-breaking-changes`: Requires API change detection in diff

## Hook Execution Rules

### Rule 1: Context Load First
Every task session must start with `pre-task.context-load`.

### Rule 2: Memory Save After Analysis
After impact/spec/implementation phases, run corresponding `post-*` hooks to save decisions.

### Rule 3: Review Before PR
Never create PR without running:
1. `pre-review.spec-check`
2. `quality.review-diff`

### Rule 4: Gates Are Hard Stops
⛔ Gates must be satisfied before proceeding:
- Spec approval gate
- Pre-PR readiness gate

### Rule 5: Use Chains for Multi-Step
For complete workflows, follow chains in `chains/` instead of executing individual hooks.

## Hook Interdependencies Table

| Hook | Requires | Required By |
|---|---|---|
| pre-task.context-load | - | All workflows |
| post-impact.memory-snapshot | Impact Agent output | Future `pre-task` calls |
| post-spec.approval-gate | Spec Agent output | Implementation start |
| pre-implementation.scope-check | Spec + target files | Implementation Agent |
| post-implementation.context-update | Implementation output | Archive purposes |
| post-implementation.test-generation | Implementation output | Test Agent |
| pre-review.spec-check | Diff + Spec | quality.review-diff |
| quality.review-diff | pre-review.spec-check pass | post-review, pre-pr |
| quality.detect-breaking-changes | API change in diff | pr.pre-pr.gate |
| post-review.findings-archive | review findings | Archive purposes |
| pre-pr.gate-readiness | All previous gates passed | PR creation |
| post-pr.changelog-update | PR merged | Archive purposes |
| context.update-patterns | New pattern discovered | Future pattern lookups |
| post-debug.memory-archive | Bug root cause identified | Future bug lookups |

## Chain Dependencies

| Chain | Start Hook | End Hook | Total Gates |
|---|---|---|---|
| new-feature | pre-task.context-load | post-pr.changelog-update | 2 |
| bug-fix | Debug Agent | post-pr.changelog-update | 0 |
| refactor | pre-task.context-load | post-pr.changelog-update | 0 |
| api-change | pre-task.context-load | post-pr.changelog-update | 2 |

## Circular Dependencies (Avoid)

These are anti-patterns:

```
❌ pre-task.context-load → spec → implementation → context-update → pre-task
   (Would cause infinite loop if context-update changes spec)

✅ Correct: context-update saves decision note, next pre-task loads note, but doesn't re-run spec
```

## No-Op Scenarios

Some hooks may output "No changes needed" or "Already compliant":

- `post-spec.approval-gate`: Spec already approved → proceed
- `quality.detect-breaking-changes`: No breaking changes → proceed
- `pre-implementation.scope-check`: Scope valid → proceed
- `pre-pr.gate-readiness`: All checks passed → create PR

These are success cases, not failures.
