# QA Hook Catalog

**For execution flow and when to use hooks, see:** [workflow-orchestrator.md](../workflow-orchestrator.md)  
**For hook format standard, see:** [HOOK-STANDARD.md](HOOK-STANDARD.md)

## Hook Inventory

| Phase | Hook | Max Input | Max Output |
|-------|------|-----------|-----------|
| Pre | `pre-task/context-load.prompt.md` | 300 | 200 |
| 2-3 | `post-impact/memory-snapshot.prompt.md` | 400 | 150 |
| 3 | `spec/post-spec.qa-analysis.prompt.md` | 700 | 300 |
| 4 | `spec/post-spec.risk-analysis.prompt.md` | 700 | 300 |
| 5 | `test/post-implementation.generate-tests.prompt.md` | 800 | 500 |
| 7 | `regression/regression-scope-update.prompt.md` | 600 | 250 |
| 8 | `security/pre-release.security-validation.prompt.md` | 900 | 350 |
| 9 | `healing/post-failure.healing-analysis.prompt.md` | 700 | 300 |
| 10 | `release/pre-release.smoke-validation.prompt.md` | 500 | 200 |
| 10 | `release/release-readiness-review.prompt.md` | 600 | 200 |
| Review | `quality/review-diff.prompt.md` | 900 | 400 |
| Review | `quality/detect-breaking-changes.prompt.md` | 800 | 300 |
| Review | `review/pre-review.spec-check.prompt.md` | 700 | 250 |
| Review | `review/post-review.findings-archive.prompt.md` | 600 | 200 |
| Context | `context/update-patterns.prompt.md` | 500 | 150 |

## Token Rules

All hooks follow strict budgets from [instructions/token-optimization/token-budgets.md](../instructions/token-optimization/token-budgets.md)

**Key principle:** Each hook operates independently within its token budget. Chain hooks by saving intermediate results to `qa-memory/`

## Execution

See [workflow-orchestrator.md](../workflow-orchestrator.md) for:
- Which hook to run in which phase
- What inputs each hook needs
- What to do when hook completes
- Where to archive outputs
