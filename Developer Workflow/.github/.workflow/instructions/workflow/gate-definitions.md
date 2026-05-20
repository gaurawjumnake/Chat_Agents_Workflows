# Gate Definitions & Execution Model

## Overview

Gates are **hard stops** in the workflow that cannot be bypassed except through emergency procedures. Each gate validates specific constraints before allowing progression to the next phase.

## Gate System Architecture

```
Entry Phase
    ↓
[Pre-conditions checked by Gate]
    ↓
├─ Gate PASSES → Proceed to next phase
├─ Gate FAILS  → Request fixes + retry
└─ Gate BLOCKED → Emergency override (logged)
```

## Core Gates

### Gate 1: Spec Approval Gate
**Location**: After Phase 3 (Specification)  
**Enforcer**: Human approver (team lead)  
**Timing**: Blocking (5 minutes - 24 hours)  
**Bypass**: Emergency only (rare)  

**Validation Checklist**:
```
✓ Summary is clear (readers understand feature)
✓ Affected files listed (all modules involved)
✓ API contracts documented (inputs/outputs/errors)
✓ Business logic explicit (algorithm or rules)
✓ Edge cases identified (boundary conditions)
✓ Test scenarios provided (at least 3 scenarios)
✓ Non-goals declared (what's NOT in scope)
✓ Breaking changes (if any) documented + migration path provided
```

**Failure Reasons**:
- "Spec too vague - which files are affected?"
- "Missing edge case: what if amount is 0?"
- "Breaking change without migration path"
- "Conflicts with approved spec X"

**Recovery**:
1. Author revises spec
2. Resubmit for approval
3. Approver re-reviews (quick cycle)
4. Approved → Continue to Phase 4

**Reference**: See [approval-rules.md](../core/approval-rules.md#1-spec-approval-gate)

---

### Gate 2: Pre-Implementation Scope Check
**Location**: Before Phase 5 (Implementation)  
**Enforcer**: Implementation Agent (automated)  
**Timing**: Immediate (no wait)  
**Bypass**: Manual override only with documentation  

**Validation Checklist**:
```
✓ Approved spec exists
✓ Target files match spec's affected-files list
✓ No undeclared files (scope creep prevention)
✓ Implementation budget available (token count check)
✓ No conflicting implementations in progress
✓ Access to all target files (permissions check)
```

**Failure Reasons**:
- "Spec affected_files missing src/payment/hooks.py"
- "You specified src/payment/ but spec only mentions src/payment/processors/"
- "Implementation token budget exceeded (1500 available, 2000 needed)"
- "Another PR already modifying src/payment/models.py"

**Recovery**:
1. Scope mismatch? → Update spec OR update target files list
2. Budget exceeded? → Split implementation into smaller PRs
3. Conflicting PR? → Wait for conflict to resolve, then retry

---

### Gate 3: Pre-Review Spec Alignment Check
**Location**: Before Phase 9 Code Review  
**Enforcer**: Review Agent (automated)  
**Timing**: Immediate  
**Bypass**: NO (hard stop)  

**Validation Checklist**:
```
✓ Code implements all spec requirements
✓ Code doesn't exceed spec scope
✓ Edge cases from spec are handled
✓ Error handling matches spec
✓ No undocumented features (no scope creep)
✓ API signatures match spec contracts
```

**Failure Reasons**:
- "Spec requires `amount` to be validated > 0, but code doesn't check"
- "Code adds new field `metadata` not mentioned in spec"
- "Error handling missing for InvalidAmountError in spec"

**Recovery**:
1. Narrow scope mismatch? → Fix code to align with spec
2. Significant mismatch? → Either:
   - Revert code + create amended spec, OR
   - Accept code as enhancement + update spec + re-approve

---

### Gate 4: Breaking Change Detection
**Location**: During Phase 9 Code Review  
**Enforcer**: Quality Agent (automated, triggered on API changes)  
**Timing**: Blocking (1-2 days for approval)  
**Bypass**: NO (hard stop)  

**Trigger Conditions**:
Breaking change automatically detected if:
- Function signature changed (parameter removed/renamed)
- Return type changed
- Behavior changed (function does different thing)
- Database schema changed (no backward compat)
- Configuration format changed

**Validation Checklist**:
```
✓ Breaking change identified in spec "Breaking Changes" section
✓ Migration path documented (how do clients adapt?)
✓ Deprecation timeline specified (if removal planned)
✓ Changelog mentions breaking change
✓ Communication plan (docs/announcement)
✓ Rollback plan (if needed)
```

**Examples**:
```
✓ ACCEPTABLE:
  - "API v2 endpoint added, v1 deprecated, 6-month sunset"
  - "Parameter reordered, but same type and default"
  - "New required parameter with default value"

✗ NOT ACCEPTABLE:
  - Removing parameter with no migration path
  - Changing return type without warning
  - Changing behavior silently
```

**Recovery**:
1. Amend spec with "Breaking Changes" section
2. Create migration guide
3. Update changelog
4. Resubmit for approval

---

### Gate 5: Security Gate
**Location**: During Phase 9 Code Review  
**Enforcer**: Security team or designated reviewer  
**Timing**: 1-2 days  
**Bypass**: NO (hard stop)  

**Triggers Automatically If**:
- Files in `/auth/`, `/security/`, `/payment/` modified
- Code mentions "secret", "password", "token", "credential", "PII", "encryption"
- SQL queries used (must be parameterized)
- External API calls made
- File system operations
- Crypto functions used

**Validation Checklist**:
```
✓ No hardcoded secrets (no password literals)
✓ User input sanitized (no SQL injection)
✓ Authentication checks present
✓ Authorization checks present (not just auth)
✓ Sensitive data not logged
✓ PII handling complies with privacy rules
✓ Secrets stored securely (not in config files)
✓ Error messages don't leak sensitive info
✓ No unencrypted transmission of sensitive data
```

**Failure Reasons**:
- "Found hardcoded API key in src/payment/gateway.py:42"
- "SQL query uses string concatenation (injection risk)"
- "PII (email) being logged without redaction"

**Recovery**:
1. Fix security issue
2. Security team re-reviews
3. Approved → Continue to next phase

---

### Gate 6: Pre-PR Readiness Check
**Location**: Before PR can be merged  
**Enforcer**: CI/CD pipeline (automated)  
**Timing**: 1-2 hours  
**Bypass**: NO (must pass all checks)  

**Validation Checklist**:
```
✓ All tests passing (100% pass rate)
✓ Code coverage >= 80%
✓ Linter passing (zero new warnings)
✓ Static analysis clean (no critical issues)
✓ Spec referenced in PR description
✓ Commit messages follow format (see coding-standards.md)
✓ Documentation complete (README, docstrings, changelog)
✓ Performance impact acceptable (<5% degradation)
✓ All previous gates passed (spec, scope, alignment, breaking change, security)
✓ Minimum approvals received (1-2 depending on change)
```

**Failure Reasons**:
- "Test coverage 72% (need 80%)"
- "3 linter warnings (ESLint)"
- "Missing changelog entry"
- "PR description doesn't cite spec"

**Recovery**:
1. Fix failing check
2. Re-run CI pipeline
3. If still fails, fix and retry
4. All checks pass → PR can merge

## Gate Execution Flow

```
User submits PR
    ↓
Gate 3: Pre-Review Spec Alignment Check
    ├─ PASS → Continue
    └─ FAIL → Author fixes code + resubmits
            → Re-run Gate 3
    ↓
Code Review (Phase 9)
    ↓
Gate 4: Breaking Change Detection (if applicable)
    ├─ PASS → Continue
    └─ FAIL → Author updates spec + migration path
            → Re-review + approval
    ↓
Gate 5: Security Gate (if applicable)
    ├─ PASS → Continue
    └─ FAIL → Author fixes security issue
            → Security team re-reviews
    ↓
Gate 6: Pre-PR Readiness Check
    ├─ PASS → Can merge
    └─ FAIL → Author fixes issues + retries
            → Re-run Gate 6
```

## Gate Parameters

### Pass/Fail Criteria

| Gate | Pass Criteria | Fail Criteria | Action |
|---|---|---|---|
| Spec Approval | All checklist ✓, Approver signs off | Any checklist ✗ | Revise → Re-review |
| Scope Check | Scope match, Budget ok | Scope mismatch, Budget exceeded | Update scope OR split work |
| Spec Alignment | Code implements spec | Code beyond/below spec | Fix code OR update spec |
| Breaking Change | Migration documented | No migration path | Document migration |
| Security | No issues found | Issues present | Fix issues → Re-review |
| Readiness | All checks ✓, Tests 100% | Any check ✗ | Fix → Re-run |

### Timing

| Gate | Immediate | Typical Duration | Max Duration | Escalation |
|---|---|---|---|---|
| Spec Approval | ❌ | 2-4 hours | 24 hours | VP if >24h |
| Scope Check | ✅ | <1 minute | <5 minutes | Auto-fail if >5m |
| Spec Alignment | ✅ | <1 minute | <5 minutes | Auto-fail if >5m |
| Breaking Change | ❌ | 2-4 hours | 24 hours | VP if >24h |
| Security | ❌ | 2-8 hours | 48 hours | CISO if >48h |
| Readiness | ✅ | 1-2 hours | <3 hours | Auto-fail if >3h |

## Gate Dependencies

```
Spec Approval Gate (must pass first)
    ↓
Pre-Implementation Scope Check (must pass)
    ↓
    ├─ [Implementation Phase 5-7 runs in parallel]
    │
    └─ Once code complete:
        ↓
        Pre-Review Spec Alignment Check (must pass)
            ↓
            Breaking Change Detection (if applicable, must pass)
                ↓
                Security Gate (if applicable, must pass)
                    ↓
                    Pre-PR Readiness Check (must pass)
                        ↓
                        PR can merge
```

## Emergency Override Procedures

### When to Override (Rare)
1. **Production outage** (users affected)
2. **Security breach** (immediate threat)
3. **Data loss incident** (recovery required)

### Override Process
```
Emergency detected
    ↓
Request override (provide justification)
    ↓
VP/CTO approval required
    ↓
Log override to memory/incidents/overrides.md
    ↓
Execute fix (bypass gate)
    ↓
24-hour post-mortem
    ↓
Add monitoring to prevent recurrence
```

### Override Template
```markdown
# Emergency Override: [Gate Name]

## Date/Time
[When override requested]

## Emergency Description
[Why is this critical?]

## Risk Assessment
[What happens if we don't override?]

## Approval
- [ ] VP approved
- [ ] CTO approved
- [ ] Incident ticket created

## Fix Applied
[What changed]

## Post-Mortem
[Scheduled for 24h later]

## Prevention
[What monitoring/fix prevents this?]
```

## Gate Metrics & Monitoring

### Metrics Tracked
- **Spec Approval Time**: Average time to approval
- **Gate Pass Rate**: % of PRs passing each gate first try
- **Bypass Rate**: % of emergency overrides
- **Recovery Time**: Average time to fix → retry
- **Gate Effectiveness**: Bug escape rate post-gate

### Weekly Report
```
Spec Approval Gate:
  - 12 specs reviewed, 11 approved on first try
  - 1 rejected (missing edge cases)
  - Avg approval time: 3 hours
  - Bypass rate: 0%

Scope Check Gate:
  - 25 implementations reviewed
  - 1 scope mismatch (author fixed)
  - Pass rate: 96%

Readiness Gate:
  - 30 PRs reviewed
  - 5 initial failures (coverage/linter)
  - All fixed and merged
  - Pass rate on second try: 100%
```

## Reference

For detailed gate mechanics, see:
- [approval-rules.md](../core/approval-rules.md) - Gate definitions & approval criteria
- [lifecycle-routing.md](lifecycle-routing.md) - When gates are triggered
- [token-budget-framework.md](../token-optimization/token-budget.md) - Token budget enforcement (part of scope check)
