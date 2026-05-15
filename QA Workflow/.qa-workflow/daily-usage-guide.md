# Daily QA Usage Guide

## Fast QA Loop

1. Run Requirement QA Agent for testability checks.
2. Run QA Impact Agent for affected scope.
3. Execute `post-spec.qa-analysis` and `post-spec.risk-analysis` hooks.
4. Generate targeted tests using `post-implementation.generate-tests`.
5. Update regression scope with `regression-scope-update`.
6. Run security validation before release (`pre-release.security-validation`).
7. If flakiness/failure occurs, run `post-failure.healing-analysis`.
8. Run smoke validation and release readiness gates.
9. Archive findings in `qa-memory/`.

## Minimal Chat Inputs

- Spec path
- Changed modules/diff summary
- Test artifacts/failures
- Environment and build identifiers

## Avoid in Prompts

- Full repository dumps
- Entire test logs if one failing slice is enough
- Repeating context already present in `qa-context/` or `qa-memory/`

## Daily Maintenance

- Update one regression intelligence note.
- Archive one bug/security/healing note when applicable.
- Prune stale flaky test entries monthly.
