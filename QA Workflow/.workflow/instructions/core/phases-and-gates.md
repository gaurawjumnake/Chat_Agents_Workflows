# QA Workflow Phases and Gates

## 10 Core Phases

| # | Phase | Key Input | Key Output | Max Context | Gate |
|---|-------|-----------|-----------|-------------|------|
| 1 | Requirement QA | Requirement note + AC | Testable summary + ambiguities | 500 | REQUIREMENT_APPROVED |
| 2 | QA Impact Analysis | Entry points + diff | Affected scope + risk hotspots | 700 | IMPACT_MAPPED |
| 3 | Spec QA Analysis | Approved spec | QA strategy + test matrix gaps | 900 | SPEC_QA_APPROVED |
| 4 | Risk & Threat Analysis | Spec + impact map | Risk register + threat model | 900 | RISK_APPROVED |
| 5 | Test Design & Execution | Spec + diff | Functional/API/automation tests | 1000 | TEST_RESULTS_READY |
| 6 | Test Execution & Debug | Failures + logs | Root cause + regression additions | 900 | FAILURES_TRIAGED |
| 7 | Regression Scope | Spec delta + modules | Targeted regression subset | 800 | REGRESSION_SCOPED |
| 8 | Security Validation | Auth/RBAC/deps | Security verdict + actions | 900 | SECURITY_PASS |
| 9 | Healing Analysis | Flaky/failing tests | Healing proposal + guardrails | 800 | HEALING_APPROVED |
| 10 | Release Validation | Smoke pack + risks | Go/No-Go decision | 700 | RELEASE_APPROVED |

## Hard Approval Gates

These gates MUST be satisfied before proceeding to next phase:

1. **REQUIREMENT_APPROVED** (after Phase 1)
   - Requirement is testable
   - Acceptance criteria are clear
   - No major ambiguities

2. **SPEC_QA_APPROVED** (after Phase 3)
   - Spec aligns with requirement
   - QA strategy is complete
   - Test matrix is defined

3. **RISK_APPROVED** (after Phase 4)
   - Risk register is complete
   - Threat model is updated
   - Security concerns identified

4. **SECURITY_PASS** (before Phase 10)
   - Auth/RBAC validated
   - Dependency security checked
   - No secrets exposed
   - Abuse paths mitigated

5. **HEALING_APPROVED** (before applying healing)
   - Root cause identified
   - Proposal is safe
   - Human approval obtained

6. **RELEASE_APPROVED** (before deployment)
   - All QA gates passed
   - Critical risks resolved
   - Release readiness confirmed

## Reversal Rules

If a gate fails:

- **REQUIREMENT fails**: Go back to requirement phase, clarify AC
- **SPEC_QA fails**: Go back to spec review, update QA strategy
- **RISK fails**: Go back to impact analysis, identify new risks
- **SECURITY fails**: Go back to threat modeling, remediate issues
- **HEALING fails**: Do not patch, file bug ticket
- **RELEASE fails**: Go back to test/security phases, resolve blockers

## Phase Dependencies

```
Requirement QA
    ↓
QA Impact Analysis
    ↓
Spec QA Analysis + Risk Analysis (parallel)
    ↓
Test Design & Execution
    ↓
Regression Scope (parallel with test execution)
    ↓
Security Validation (parallel)
    ↓
Healing Analysis (if needed)
    ↓
Release Validation
```

## Key Rules

- **Spec-first**: Always reference spec path, avoid repeating raw requirements
- **Diff-focused**: Use diffs and changed modules, not full repo scans
- **No release without security**: Security validation is mandatory
- **Memory discipline**: Archive every major QA decision in `qa-memory/`
- **Human approval required**: Security and healing outputs need explicit sign-off
