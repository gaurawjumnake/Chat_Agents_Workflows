# QA Hook Dependency Graph

## Primary Order

1. pre-task.context-load
2. post-impact.memory-snapshot
3. spec.approval-gate
4. post-spec.qa-analysis
5. post-spec.risk-analysis
6. post-implementation.generate-tests
7. regression-scope-update
8. pre-review.spec-check
9. quality.review-diff
10. pre-release.security-validation
11. pre-release.smoke-validation
12. release-readiness-review

## Conditional Branches

- If flaky/unstable failure is detected: insert `post-failure.healing-analysis` before release hooks.
- If breaking changes detected: run migration validations before release readiness.

## Hard Gates

- spec.approval-gate
- post-spec.risk-analysis (risk review required when blocked)
- pre-release.security-validation
- post-failure.healing-analysis (human approval required)
- release-readiness-review
