# QA Workflow Migration Summary

## 1. Migration Summary

Developer workflow under `.github/.workflow` was converted to QA-focused workflow under `.github/.qa-workflow` by preserving shared framework assets and replacing implementation-centric roles with validation-centric roles.

## 2. Files Kept (Conceptually Reused)

- Shared Spec-DD structure and gate philosophy
- Token optimization strategy and checklists
- Hook execution model and IDE chat compatibility
- Specs compatibility (`specs/` template and references)
- Memory pattern (persistent notes, reusable artifact indexing)

## 3. Files Removed (From QA Variant)

- Implementation-centric agent behaviors
- Design/architecture generation flow
- Refactor-focused flow
- Code-generation snippets (`implement_from_spec` style)
- Developer-only implementation hooks

## 4. Files Converted

- `workflow.md` converted from implementation phases to QA validation phases
- Agent set converted to QA roles
- Hook catalog converted to QA hooks and QA gates
- Daily guide converted to QA execution loop
- Snippets converted to QA tasks (test generation, risk, security, healing, release)

## 5. New QA Files Created

- `qa-context/*` strategy and environment intelligence files
- `qa-memory/security/`, `qa-memory/healing/`, `qa-memory/regression/`
- QA-specific hooks under `hooks/test`, `hooks/security`, `hooks/healing`, `hooks/release`, `hooks/regression`
- QA snippets under `snippets/qa/`

## 6. Updated Folder Structure

See `README.md` and `hooks/CATALOG.md` for the practical map.

## 7. Updated Agents

- Requirement QA Agent
- QA Impact Agent
- Risk Agent
- Functional Test Agent
- API Test Agent
- Regression Agent
- Security Agent
- Threat Model Agent
- Healing Agent
- Bug Analysis Agent
- Release Validation Agent

## 8. Updated Snippets

Added QA snippets for:
- test generation
- edge-case generation
- regression scope analysis
- security validation
- healing analysis
- release validation
- bug analysis
- API contract testing

## 9. Updated Hooks

Added/converted QA hooks:
- `spec/post-spec.qa-analysis.prompt.md`
- `spec/post-spec.risk-analysis.prompt.md`
- `test/post-implementation.generate-tests.prompt.md`
- `regression/regression-scope-update.prompt.md`
- `security/pre-release.security-validation.prompt.md`
- `healing/post-failure.healing-analysis.prompt.md`
- `release/pre-release.smoke-validation.prompt.md`
- `release/release-readiness-review.prompt.md`

## 10. Security Workflow

Modeled in:
- `qa-context/security-model.md`
- `hooks/security/pre-release.security-validation.prompt.md`
- `agents/security.agent.md`
- `agents/threat-model.agent.md`

Includes auth, RBAC, API abuse, dependency checks, secret exposure checks, and threat modeling updates.

## 11. Healing Workflow

Modeled in:
- `qa-context/flaky-tests.md`
- `qa-memory/healing/README.md`
- `hooks/healing/post-failure.healing-analysis.prompt.md`
- `agents/healing.agent.md`

Enforces root-cause evidence and human approval before any patch action.

## 12. Regression Workflow

Modeled in:
- `qa-context/regression-map.md`
- `hooks/regression/regression-scope-update.prompt.md`
- `agents/regression.agent.md`

Uses changed modules, dependencies, and spec deltas to recommend targeted regression suites.

## 13. Token Optimization Improvements

- Spec references instead of requirement repetition
- Diff-based QA validation instead of full file scans
- Incremental context and memory updates only
- Hook and snippet reuse to avoid ad-hoc verbose prompts
- Gate prompts constrained to compact structured outputs
