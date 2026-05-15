# QA Hook-Based Execution Model

Hooks are workflow triggers, not runtime code.

## Pre-Task Context Hook

Load relevant spec, QA context, and memory notes.
Output a compact context packet.

## Post-Impact Snapshot Hook

Persist impacted modules, risk hotspots, and suggested suites.

## Post-Spec QA Analysis Hook

Derive QA strategy coverage and missing scenarios from spec.

## Post-Spec Risk Analysis Hook

Build risk register and block conditions.

## Test Generation Hook

Generate targeted functional/API/regression/security tests.

## Regression Scope Hook

Compute minimal regression subset from changed modules and dependencies.

## Security Validation Hook

Run auth/RBAC/abuse/dependency/secret checks before release.

## Healing Analysis Hook

Analyze flakiness and propose healing only with human approval.

## Release Hooks

- Smoke validation
- Final readiness review and go/no-go gate
