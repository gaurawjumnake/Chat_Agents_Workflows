# Approval Gate Rules

## Approval Gate System

All hard gates require explicit human decision before proceeding.

## Gate Lifecycle

```
Gate Triggered
    ↓
Collect Evidence
    ↓
Human Review
    ↓
DECISION: Approved / Rejected / Conditional
    ↓
(If Approved)
    ↓
Archive Approval + Rationale
    ↓
Proceed to Next Phase
```

## Approval Evidence Requirements

### REQUIREMENT_APPROVED

**Evidence Required**:
- Acceptance criteria clearly stated
- Testable conditions defined
- Dependencies identified
- Edge cases acknowledged

**Approval Form**:
```
Gate: REQUIREMENT_APPROVED
Spec: specs/<feature>.md
Approval: [Yes / No / Conditional]
Conditions (if any): [list]
Approver: [role]
Date: [date]
```

### SPEC_QA_APPROVED

**Evidence Required**:
- Spec completeness confirmed
- QA coverage strategy defined
- Test matrix reviewed
- Missing coverage acknowledged

**Approval Form**:
```
Gate: SPEC_QA_APPROVED
Spec: specs/<feature>.md
Coverage gaps (if any): [list]
Approval: [Yes / No / Conditional]
Approver: [role]
Date: [date]
```

### RISK_APPROVED

**Evidence Required**:
- Risk register completed
- Threat model updated
- Mitigation strategies defined
- Go/No-Go decision on risk level

**Approval Form**:
```
Gate: RISK_APPROVED
Risk level: [Low / Medium / High / Critical]
Unacceptable risks: [none / list]
Approval: [Yes / No]
Approver: [role]
Date: [date]
```

### SECURITY_PASS

**Evidence Required**:
- Auth/RBAC validation done
- Dependency security scanned
- Secret exposure checked
- Abuse paths documented

**Approval Form**:
```
Gate: SECURITY_PASS
Vulnerabilities found: [none / list]
Remediation plan: [if needed]
Approval: [Yes / No / Conditional]
Approver: [security role]
Date: [date]
```

### HEALING_APPROVED

**Evidence Required**:
- Root cause identified with logs/metrics
- Flakiness pattern documented
- Healing proposal is safe and reversible
- Test coverage for healing included

**Approval Form**:
```
Gate: HEALING_APPROVED
Root cause: [documented]
Healing action: [specific]
Reversibility: [yes / no]
Safety guardrails: [list]
Approval: [Yes / No]
Approver: [technical lead]
Date: [date]
```

### RELEASE_APPROVED

**Evidence Required**:
- All prior gates passed
- Critical test results passed
- Smoke validation passed
- Release readiness confirmed
- No unresolved high-risk issues

**Approval Form**:
```
Gate: RELEASE_APPROVED
All gates passed: [yes / no]
Critical issues resolved: [yes / no]
Smoke validation: [pass / fail]
Deployment readiness: [ready / blocked]
Approval: [Yes / No]
Approver: [release manager]
Date: [date]
```

## Conditional Approvals

A conditional approval means: "Approved IF [condition] is met"

**Examples**:
- SPEC_QA_APPROVED (conditional on fixing coverage gap X)
- RELEASE_APPROVED (conditional on resolving issue #123)

Conditions must be:
1. Specific and testable
2. Have a clear owner
3. Have a deadline
4. Be tracked in a decision log

## Rejection Handling

If a gate is rejected:

1. Document rejection reason with evidence
2. Identify what needs to change
3. Go back to relevant phase
4. Update affected artifacts
5. Re-request approval

## Approval Record Keeping

All approvals should be archived in:
- `qa-memory/approvals/` (one file per gate+feature)
- Gate record includes: decision, evidence, approver, date, conditions

## Conditional vs. Unconditional

- **Unconditional**: "This change is APPROVED to proceed to the next phase"
- **Conditional**: "This change is APPROVED TO PROCEED IF X is resolved within [deadline]"

Conditional approvals require a follow-up gate once conditions are met.
