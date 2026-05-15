# Developer-to-QA File Classification

This file classifies each source asset from `.github/.workflow` as KEEP, REMOVE, CONVERT, or CREATE (QA target).

## Top-Level Files

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/README.md` | CONVERT | `.github/.qa-workflow/README.md` |
| `.github/.workflow/workflow.md` | CONVERT | `.github/.qa-workflow/workflow.md` |
| `.github/.workflow/daily-usage-guide.md` | CONVERT | `.github/.qa-workflow/daily-usage-guide.md` |
| `.github/.workflow/token-optimization-checklist.md` | CONVERT | `.github/.qa-workflow/token-optimization-checklist.md` |

## Agents

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/agents/requirement.agent.md` | CONVERT | `agents/requirement-qa.agent.md` |
| `.github/.workflow/agents/impact.agent.md` | CONVERT | `agents/qa-impact.agent.md` |
| `.github/.workflow/agents/spec.agent.md` | REMOVE | Replaced by spec hooks + requirement QA flow |
| `.github/.workflow/agents/design.agent.md` | REMOVE | Not needed in validation-centric workflow |
| `.github/.workflow/agents/implementation.agent.md` | REMOVE | Not needed in validation-centric workflow |
| `.github/.workflow/agents/test.agent.md` | CONVERT | `agents/functional-test.agent.md`, `agents/api-test.agent.md` |
| `.github/.workflow/agents/debug.agent.md` | CONVERT | `agents/bug-analysis.agent.md`, `agents/healing.agent.md` |
| `.github/.workflow/agents/review.agent.md` | CONVERT | `agents/risk.agent.md`, QA review hooks |
| `.github/.workflow/agents/documentation.agent.md` | REMOVE | Release validation output supersedes doc generation role |
| N/A | CREATE | `agents/regression.agent.md` |
| N/A | CREATE | `agents/security.agent.md` |
| N/A | CREATE | `agents/threat-model.agent.md` |
| N/A | CREATE | `agents/release-validation.agent.md` |

## Snippets

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/snippets/explain_code.snippet.md` | KEEP | Optional shared usage (not required in QA pack) |
| `.github/.workflow/snippets/impact_analysis.snippet.md` | CONVERT | `snippets/qa/regression_analysis.snippet.md` |
| `.github/.workflow/snippets/generate_tests.snippet.md` | CONVERT | `snippets/qa/test_generation.snippet.md` |
| `.github/.workflow/snippets/debug_issue.snippet.md` | CONVERT | `snippets/qa/bug_analysis.snippet.md`, `snippets/qa/healing_analysis.snippet.md` |
| `.github/.workflow/snippets/review_diff.snippet.md` | CONVERT | Hook-based QA diff review |
| `.github/.workflow/snippets/create_spec.snippet.md` | KEEP | Spec compatibility maintained via `specs/spec-template.md` |
| `.github/.workflow/snippets/implement_from_spec.snippet.md` | REMOVE | Implementation-centric |
| N/A | CREATE | `snippets/qa/edge_case_generation.snippet.md` |
| N/A | CREATE | `snippets/qa/security_validation.snippet.md` |
| N/A | CREATE | `snippets/qa/release_validation.snippet.md` |
| N/A | CREATE | `snippets/qa/api_contract_test.snippet.md` |

## Hooks: Core Docs

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/hooks/README.md` | CONVERT | `hooks/README.md` |
| `.github/.workflow/hooks/CATALOG.md` | CONVERT | `hooks/CATALOG.md` |
| `.github/.workflow/hooks/hook-model.md` | CONVERT | `hooks/hook-model.md` |
| `.github/.workflow/hooks/IDE-EXECUTION-GUIDE.md` | CONVERT | `hooks/IDE-EXECUTION-GUIDE.md` |
| `.github/.workflow/hooks/TOKEN-OPTIMIZATION-STRATEGY.md` | CONVERT | `hooks/TOKEN-OPTIMIZATION-STRATEGY.md` |
| `.github/.workflow/hooks/chains/README.md` | CONVERT | `hooks/chains/README.md` |
| `.github/.workflow/hooks/DAILY-USAGE-EXAMPLES.md` | CONVERT | Covered by `.github/.qa-workflow/daily-usage-guide.md` |
| `.github/.workflow/hooks/INTEGRATION-GUIDE.md` | KEEP | Reusable pattern; QA flow remains compatible |
| `.github/.workflow/hooks/DEPENDENCY-GRAPH.md` | CONVERT | Hook chain dependencies in `hooks/chains/README.md` |
| `.github/.workflow/hooks/DELIVERY-SUMMARY.md` | REMOVE | Historical delivery artifact |
| `.github/.workflow/hooks/QUICK-START.md` | KEEP | Content merged into `hooks/README.md` |

## Hooks: Prompt Templates

| Source File | Decision | QA Target |
|---|---|---|
| `hooks/pre-task/context-load.prompt.md` | CONVERT | `hooks/pre-task/context-load.prompt.md` |
| `hooks/post-impact/memory-snapshot.prompt.md` | CONVERT | `hooks/post-impact/memory-snapshot.prompt.md` |
| `hooks/spec/post-requirement.auto-create-spec.prompt.md` | REMOVE | Developer spec drafting workflow; QA uses post-spec analysis |
| `hooks/spec/approval-gate.prompt.md` | KEEP/CONVERT | `hooks/spec/approval-gate.prompt.md` |
| `hooks/implementation/pre-implementation.scope-check.prompt.md` | REMOVE | Implementation-centric |
| `hooks/implementation/post-implementation.context-update.prompt.md` | REMOVE | Implementation-centric |
| `hooks/implementation/post-implementation.test-generation.prompt.md` | CONVERT | `hooks/test/post-implementation.generate-tests.prompt.md` |
| `hooks/review/pre-review.spec-check.prompt.md` | CONVERT | `hooks/review/pre-review.spec-check.prompt.md` |
| `hooks/review/post-review.findings-archive.prompt.md` | CONVERT | `hooks/review/post-review.findings-archive.prompt.md` |
| `hooks/quality/review-diff.prompt.md` | CONVERT | `hooks/quality/review-diff.prompt.md` |
| `hooks/quality/detect-breaking-changes.prompt.md` | CONVERT | `hooks/quality/detect-breaking-changes.prompt.md` |
| `hooks/debug/post-debug.memory-archive.prompt.md` | CONVERT | `hooks/healing/post-failure.healing-analysis.prompt.md` + `qa-memory/bugs/*` |
| `hooks/context/update-patterns.prompt.md` | KEEP/CONVERT | `hooks/context/update-patterns.prompt.md` |
| `hooks/pr/pre-pr.gate-readiness.prompt.md` | CONVERT | `hooks/release/release-readiness-review.prompt.md` |
| `hooks/pr/post-pr.changelog-update.prompt.md` | REMOVE | QA release gate replaces changelog automation focus |
| N/A | CREATE | `hooks/spec/post-spec.qa-analysis.prompt.md` |
| N/A | CREATE | `hooks/spec/post-spec.risk-analysis.prompt.md` |
| N/A | CREATE | `hooks/regression/regression-scope-update.prompt.md` |
| N/A | CREATE | `hooks/security/pre-release.security-validation.prompt.md` |
| N/A | CREATE | `hooks/release/pre-release.smoke-validation.prompt.md` |

## AI Memory and Specs

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/ai-memory/README.md` | CONVERT | `qa-memory/README.md` |
| `.github/.workflow/ai-memory/bugs/bug-template.md` | CONVERT | `qa-memory/bugs/bug-template.md` |
| `.github/.workflow/ai-memory/decisions/decision-template.md` | REMOVE | Replaced by domain-specific QA memory templates |
| `.github/.workflow/ai-memory/patterns/pattern-template.md` | CONVERT | `qa-memory/patterns/pattern-template.md` |
| `.github/.workflow/ai-memory/snippets/snippet-note-template.md` | CONVERT | `qa-memory/snippets/snippet-note-template.md` |
| `.github/.workflow/ai-memory/specs/*` | KEEP | Supported through `qa-memory/specs/` and `specs/` compatibility |
| `.github/.workflow/specs/README.md` | KEEP/CONVERT | `.github/.qa-workflow/specs/README.md` |
| `.github/.workflow/specs/spec-template.md` | KEEP/CONVERT | `.github/.qa-workflow/specs/spec-template.md` |

## Examples and Workflow Support

| Source File | Decision | QA Target |
|---|---|---|
| `.github/.workflow/examples/end-to-end-flow.md` | CONVERT | `.github/.qa-workflow/examples/end-to-end-flow.md` |
| `.github/.workflow/specs/README.md` | KEEP | Included with QA-compatible wording |

## QA-Only Created Assets

- `qa-context/test-strategy.md`
- `qa-context/regression-map.md`
- `qa-context/api-test-matrix.md`
- `qa-context/security-model.md`
- `qa-context/flaky-tests.md`
- `qa-context/environments.md`
- `qa-memory/security/*`
- `qa-memory/healing/*`
- QA-specific snippets and agents listed above
