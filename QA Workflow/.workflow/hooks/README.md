# QA Hooks Directory

Hooks are triggered execution points with approval gates.

**For hook format standard, see:** [HOOK-STANDARD.md](HOOK-STANDARD.md)  
**For execution flow, see:** [workflow-orchestrator.md](../workflow-orchestrator.md)  
**For detailed catalog, see:** [CATALOG.md](CATALOG.md)

## Hooks by Phase

**Pre-Execution**
- `pre-task/context-load.prompt.md` — Load context for phase

**Phase 3-4 (Spec & Risk Analysis)**
- `spec/post-spec.qa-analysis.prompt.md`
- `spec/post-spec.risk-analysis.prompt.md`

**Phase 5 (Test Design)**
- `test/post-implementation.generate-tests.prompt.md`

**Phase 7 (Regression)**
- `regression/regression-scope-update.prompt.md`

**Phase 8 (Security)**
- `security/pre-release.security-validation.prompt.md`

**Phase 9 (Healing)**
- `healing/post-failure.healing-analysis.prompt.md`

**Phase 10 (Release)**
- `release/pre-release.smoke-validation.prompt.md`
- `release/release-readiness-review.prompt.md`

**Quality & Review**
- `quality/review-diff.prompt.md`
- `quality/detect-breaking-changes.prompt.md`
- `review/pre-review.spec-check.prompt.md`
- `review/post-review.findings-archive.prompt.md`

**Context & Memory**
- `post-impact/memory-snapshot.prompt.md`
- `context/update-patterns.prompt.md`

---

**See [workflow-orchestrator.md](../workflow-orchestrator.md) for which hook to run in which phase.**
