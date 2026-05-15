# QA Hook IDE Execution Guide

## Single Hook Execution

```text
Execute hook: hooks/security/pre-release.security-validation

Spec: specs/<feature>.md
Changed modules: <paths>
Security model: qa-context/security-model.md
```

## Chain Execution

```text
Execute chain: chains/release-validation
Feature: <feature-name>
```

## Common Sequences

After implementation:
1. `test/post-implementation.generate-tests`
2. `regression/regression-scope-update`
3. `review/pre-review.spec-check`
4. `quality/review-diff`
5. `security/pre-release.security-validation`
6. `release/pre-release.smoke-validation`
7. `release/release-readiness-review`

Failure handling:
1. `review/post-review.findings-archive`
2. `healing/post-failure.healing-analysis` (when flaky)
