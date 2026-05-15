# Flaky Test Intelligence

## Flaky Categories

- Selector instability
- Timing/race instability
- Environment/config drift
- API schema mismatch
- Data dependency contamination

## Investigation Rules

- Reproduce first, then classify cause.
- Record confidence and evidence.
- Prefer root-cause fixes over retries.

## Healing Rule

No healing patch is applied without human approval and rollback plan.
