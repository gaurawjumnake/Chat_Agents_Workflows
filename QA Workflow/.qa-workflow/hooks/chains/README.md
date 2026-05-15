# QA Hook Chains

## Chain: qa-validation

1. pre-task.context-load
2. post-impact.memory-snapshot
3. post-spec.qa-analysis
4. post-spec.risk-analysis
5. post-implementation.generate-tests
6. regression-scope-update
7. pre-review.spec-check
8. quality.review-diff
9. pre-release.security-validation
10. pre-release.smoke-validation
11. release-readiness-review

## Chain: bug-to-release

1. pre-task.context-load
2. post-failure.healing-analysis (if flaky suspected)
3. post-implementation.generate-tests (regression additions)
4. regression-scope-update
5. pre-release.security-validation
6. release-readiness-review

## Chain: security-hotfix

1. post-impact.memory-snapshot
2. post-spec.risk-analysis
3. pre-release.security-validation
4. pre-release.smoke-validation
5. release-readiness-review

Gate rules apply for risk, security, healing, and release.
