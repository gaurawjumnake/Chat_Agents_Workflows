# Example End-to-End Flow

## Scenario
Add idempotency support to `POST /payments`.

## Step-by-step with Gates

1. Requirement phase
- Run Requirement Agent with objective, constraints, acceptance criteria.
- Output: concise requirement summary.
- Gate: user confirms.

2. Impact phase
- Run Impact Agent with endpoint and requirement summary.
- Output: impacted modules only (`payments/api`, `payments/service`, `tests/payments`).
- Gate: user confirms.

3. Spec phase
- Run Spec Agent and save `specs/payment-idempotency.md`.
- Include contract updates and test scenarios.
- Gate: mandatory approval.

4. Design phase
- Run Design Agent with approved spec path.
- Output: request key strategy, storage TTL, conflict behavior.

5. Implementation phase
- Run Implementation Agent with spec path + target files.
- Output: minimal patch only.
- Gate: confirm before merge-stage tasks.

6. Testing phase
- Run Test Agent with changed diff + spec scenarios.
- Add unit and integration tests for duplicate request behavior.

7. Debugging phase (if needed)
- Run Debug Agent with failing test and small snippet.
- Save bug note in `ai-memory/bugs/` after fix.

8. Review + PR phase
- Run Review Agent using `snippets/review_diff.snippet.md`.
- Output findings, required fixes, merge readiness.
- Gate: pre-PR confirm.

9. Documentation phase
- Run Documentation Agent to update API docs and changelog.

## Token-Safe Data Passed Per Step

- Requirement: <= 500 tokens
- Impact: <= 700 tokens
- Spec: <= 900 tokens
- Implementation: <= 1200 tokens
- Review: <= 900 tokens

## Stored Artifacts

- Spec: `specs/payment-idempotency.md`
- Decision note: `ai-memory/decisions/2026-05-14-impact-payment-idempotency.md`
- Bug note: `ai-memory/bugs/<date>-<key>.md` (if bug found)
