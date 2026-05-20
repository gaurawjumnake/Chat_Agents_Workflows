# QA End-to-End Flow Example

## Scenario
Validate release readiness for a changed payment retry API.

1. Requirement QA
- Run Requirement QA Agent.
- Gate: confirm testable acceptance criteria.

2. QA Impact
- Run QA Impact Agent.
- Output: affected modules and test scope.

3. Spec QA + Risk
- Run `spec/post-spec.qa-analysis`.
- Run `spec/post-spec.risk-analysis`.
- Gate: approve risk register.

4. Test Generation and Execution
- Run `test/post-implementation.generate-tests`.
- Execute targeted tests and record failures.

5. Regression Intelligence
- Run `regression/regression-scope-update`.
- Execute recommended subset.

6. Security Validation
- Run `security/pre-release.security-validation`.
- Gate: security pass required.

7. Healing (if failures are flaky)
- Run `healing/post-failure.healing-analysis`.
- Gate: human approval before patching.

8. Release Validation
- Run `release/pre-release.smoke-validation`.
- Run `release/release-readiness-review`.
- Gate: Go/No-Go.

## Stored Artifacts

- Regression map updates: `qa-context/regression-map.md`
- Security findings: `qa-memory/security/`
- Healing analyses: `qa-memory/healing/`
- Bug analyses: `qa-memory/bugs/`
