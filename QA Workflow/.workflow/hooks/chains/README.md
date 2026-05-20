# Hook Chains (Optional Patterns)

**For complete execution flow, see:** [workflow-orchestrator.md](../../workflow-orchestrator.md)

Hook chains are optional quick-reference patterns for common scenarios.  
They show which hooks run in sequence, but **the orchestrator defines the complete flow.**

## Scenario 1: Full QA Validation

From requirement to release:

```
1. Requirement QA (Phase 1)
2. QA Impact (Phase 2)
3. Spec QA + Risk (Phase 3-4)
4. Test Design (Phase 5)
5. Regression Scope (Phase 7)
6. Security Validation (Phase 8)
7. Release Validation (Phase 10)
```

**See:** [workflow-orchestrator.md](../../workflow-orchestrator.md) for full details

## Scenario 2: Bug to Release

When fixing bugs found during testing:

```
1. Analyze failure (Phase 6)
2. If flaky: Healing analysis (Phase 9)
3. Generate regression tests (Phase 5)
4. Update regression scope (Phase 7)
5. Security validation (Phase 8)
6. Release validation (Phase 10)
```

**See:** [workflow-orchestrator.md](../../workflow-orchestrator.md#phase-6-test-execution--debug)

## Scenario 3: Security Hotfix

For urgent security issues:

```
1. Risk & Threat analysis (Phase 4)
2. Security validation (Phase 8)
3. Smoke validation (Phase 10)
4. Release decision (Phase 10)
```

**See:** [workflow-orchestrator.md](../../workflow-orchestrator.md#phase-8-security-validation)

---

**Use the orchestrator as your primary reference for phase execution.**  
These chains are just common patterns; the orchestrator is the source of truth.
