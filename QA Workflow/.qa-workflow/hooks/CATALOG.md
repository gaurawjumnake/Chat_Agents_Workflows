# QA Hook Catalog

## Hook List

```
.github/.qa-workflow/hooks/
  pre-task/
    context-load.prompt.md
  post-impact/
    memory-snapshot.prompt.md
  spec/
    post-spec.qa-analysis.prompt.md
    post-spec.risk-analysis.prompt.md
    approval-gate.prompt.md
  test/
    post-implementation.generate-tests.prompt.md
  regression/
    regression-scope-update.prompt.md
  security/
    pre-release.security-validation.prompt.md
  healing/
    post-failure.healing-analysis.prompt.md
  review/
    pre-review.spec-check.prompt.md
    post-review.findings-archive.prompt.md
  quality/
    review-diff.prompt.md
    detect-breaking-changes.prompt.md
  release/
    pre-release.smoke-validation.prompt.md
    release-readiness-review.prompt.md
  context/
    update-patterns.prompt.md
  chains/
    README.md
```

## Token Budgets (Strict)

| Hook Type | Max Input Tokens | Max Output Tokens |
|---|---:|---:|
| pre-task | 300 | 200 |
| post-impact | 400 | 150 |
| post-spec.qa-analysis | 700 | 300 |
| post-spec.risk-analysis | 700 | 300 |
| test-generation | 800 | 500 |
| regression-scope | 600 | 250 |
| security-validation | 900 | 350 |
| healing-analysis | 700 | 300 |
| release smoke | 500 | 200 |
| release readiness | 600 | 200 |
| review-diff | 900 | 400 |
