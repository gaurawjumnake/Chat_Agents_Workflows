# Regression Map

## Objective
Map changed modules and contracts to a targeted regression subset.

## Update Inputs

- Spec deltas
- Changed modules and dependencies
- Historical defects and flaky hotspots
- Critical execution paths

## Output Format

- Must-run regression suites
- Must-run smoke tests
- Optional deferred suites with rationale

## Guardrail

Do not default to full regression unless risk score or uncertainty is high.
