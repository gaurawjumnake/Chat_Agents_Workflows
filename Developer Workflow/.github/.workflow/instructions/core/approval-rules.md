# Approval Rules & Gates System

## Gate Types

### 1. **Spec Approval Gate**
**When**: After spec is drafted by Spec Agent  
**Who**: Team lead or designated approver  
**Duration**: Up to 24 hours  
**Bypass**: NO - Cannot proceed without approval  

**Approval Checklist**:
- ✓ Summary section clear and complete
- ✓ Affected files list accurate
- ✓ API contracts (inputs/outputs) documented
- ✓ Business logic explicitly stated
- ✓ Edge cases identified and documented
- ✓ Test scenarios cover happy path + edge cases
- ✓ Non-goals section addresses common confusion
- ✓ No breaking changes OR breaking changes documented with migration path

**Rejection Reasons**:
- Missing scope clarity (unclear what files change)
- Insufficient edge case coverage
- Breaking change without migration plan
- Conflicting with other approved specs
- Resource constraints (estimated effort too high)

**Approval Output**:
- Spec marked as "APPROVED"
- Comment with approver name + date
- Reference stored in memory

---

### 2. **Pre-Implementation Scope Check Gate**
**When**: Before Implementation Agent starts coding  
**Who**: Implementation Agent (automated)  
**Duration**: Immediate  
**Bypass**: NO - Manual override possible but logged  

**Scope Checklist**:
- ✓ Approved spec path provided
- ✓ Target files match spec's affected files list
- ✓ No additional files modified (scope creep prevention)
- ✓ Implementation budget (token count) within limits
- ✓ No conflicting changes in progress

**Rejection Triggers**:
- Scope mismatch (missing files from spec)
- Spec not approved
- Implementation budget exceeded
- Previous implementation still in progress (prevent overlaps)

**Rejection Action**:
- Report specific scope mismatch
- Ask user to reconcile target files with spec
- Offer to auto-update spec if scope needs adjustment

---

### 3. **Pre-Review Spec Alignment Gate**
**When**: Before Review Agent conducts code review  
**Who**: Review Agent (automated)  
**Duration**: Immediate  
**Bypass**: NO  

**Alignment Checklist**:
- ✓ Spec path referenced in diff/PR
- ✓ Code changes match spec requirements
- ✓ No features beyond spec scope
- ✓ Edge cases from spec implemented correctly
- ✓ Error handling covers spec scenarios

**Rejection Triggers**:
- Code beyond spec scope (scope creep)
- Missing spec requirements
- Different behavior than spec describes
- Edge case handling incomplete

**Rejection Action**:
- List specific misalignments
- Suggest code changes to align with spec
- OR suggest spec amendments (requires re-approval)

---

### 4. **Breaking Change Detection Gate**
**When**: During Review phase (triggered automatically if API changes detected)  
**Who**: Quality/Security agent  
**Duration**: Immediate → 24 hours for approval  
**Bypass**: NO - Must have explicit approval  

**Breaking Change Checklist**:
- ✓ Change identified as breaking
- ✓ Documented in spec "Breaking Changes" section
- ✓ Migration path provided (for client code)
- ✓ Deprecation notice issued (if applicable)
- ✓ Timeline for removal specified (if removal planned)
- ✓ Communication plan (changelog, documentation)

**Examples of Breaking Changes**:
- Parameter removed from function
- Return type changed
- Behavior change (function does different thing)
- Database schema change (no backward compat column)
- Configuration key removed

**Non-Breaking Changes**:
- New parameters (with defaults)
- New error types (subset of old behavior)
- Performance improvements (behavior same)
- New features (opt-in)

---

### 5. **Security Review Gate**
**When**: If code handles PII, secrets, auth, or encryption  
**Who**: Security team or designated reviewer  
**Duration**: 1-2 days  
**Bypass**: NO - Cannot merge without approval  

**Security Checklist**:
- ✓ No hardcoded credentials/secrets
- ✓ User input validated/sanitized
- ✓ SQL queries parameterized (no string concat)
- ✓ Authentication/authorization checks present
- ✓ Sensitive data not logged
- ✓ PII handling complies with privacy policy
- ✓ No unencrypted storage of secrets
- ✓ Error messages don't leak sensitive info

**Triggered by**:
- Code path includes `/auth/`, `/security/`, `/payment/`, `/credentials/`
- Comments mention "secret", "password", "token", "PII", "encryption"
- Functions with parameters like `password`, `api_key`, `ssn`, `credit_card`

**Rejection Triggers**:
- Hardcoded credentials found
- SQL injection vulnerability
- Insufficient input validation
- PII mishandling

---

### 6. **Pre-PR Readiness Gate**
**When**: Before PR can be merged  
**Who**: Automated (CI/CD pipeline)  
**Duration**: 1-2 hours  
**Bypass**: NO - Must pass all checks  

**Readiness Checklist**:
- ✓ All tests passing (100% pass rate required)
- ✓ Code coverage >= 80%
- ✓ Linter/formatter passing (zero warnings)
- ✓ Static analysis clean (no critical issues)
- ✓ Documentation updated (README, docstrings, changelog)
- ✓ Spec referenced in PR description
- ✓ Commit messages follow format (see coding-standards.md)
- ✓ Pre-review spec alignment gate passed
- ✓ Security gate passed (if applicable)
- ✓ Performance impact acceptable (<5% degradation)

**PR Description Template**:
```markdown
## Spec
[Link to approved spec or spec path]

## Summary
[Brief description of changes]

## Changes
- [File]: [What changed]
- [File]: [What changed]

## Testing
- [Test scenario]: [Expected result]
- [Edge case]: [How tested]

## Breaking Changes
[If any, link to migration path]

## Deployment Notes
[Any special deployment steps?]
```

**Failure Modes**:
- Test failure → Fix code + re-run tests
- Coverage gap → Add test scenarios
- Linter warning → Fix formatting
- Documentation incomplete → Add docs + re-run gate

---

## Gate Bypass Procedures

### When Bypass is Allowed
1. **Emergency fixes** (production outage)
   - Requires VP+ approval
   - Logged with incident ticket
   - Must document in memory
   - Post-hoc review within 24 hours

2. **Technical debt reduction** (for internal code)
   - Requires team lead approval
   - Must cite ticket/issue ID
   - Must document rationale
   - Cannot bypass spec gate

### Bypass Request Template
```
Gate: [which gate]
Reason: [why bypass needed]
Risk Assessment: [what could go wrong]
Approval: [who approved]
Ticket ID: [incident/ticket reference]
Timeline: [when must this be fixed]
Post-Review: [when will this be audited]
```

### Bypass Audit Trail
- All bypasses logged to `memory/incidents/bypasses.md`
- Reviewed quarterly
- Patterns analyzed (are certain gates too strict?)

---

## Escalation & Appeals

### Appeal Process
1. **Author disagrees with gate rejection**
   - Document specific objection
   - Provide counter-evidence
   - Submit to tech lead

2. **Tech lead reviews**
   - Validates gate logic
   - May override if technical justification strong
   - Documents decision in PR comments

3. **Override logging**
   - Added to `memory/incidents/appeals.md`
   - Reviewed quarterly
   - Used to improve gate rules

### Escalation Scenarios

**Scenario 1: Disagreement on spec completeness**
```
Author: "Spec is complete enough"
Tech Lead: Reviews spec using checklist
Decision: Either approve spec or require clarification
```

**Scenario 2: Performance budget exceeded**
```
Author: "Breaking change unavoidable"
Tech Lead: 
  - Reviews impact assessment
  - Approves if impact <10% OR approves deprecation plan
Decision: Conditional approval (with migration timeline)
```

**Scenario 3: Security concern**
```
Reviewer: "Insufficient input validation"
Author: "Field already validated upstream"
Tech Lead: Reviews upstream validation, decides if sufficient
Decision: Approve with documented assumption OR require explicit validation
```

---

## Gate Metrics & Monitoring

### Metrics Tracked
- **Spec Approval Time**: Average time from draft → approval (target: <24h)
- **Gate Bypass Rate**: % of bypasses per month (target: <5%)
- **Appeal Rate**: % of appeals per month (target: <3%)
- **Rejection Rate**: % of rejections per gate (target: <10%)

### Monthly Review
- Analyze rejection patterns
- Identify gates that are too strict/loose
- Adjust rules as needed
- Update team on trends

### Reporting
- Dashboard: [gate metrics for last 30 days]
- Trend analysis: [comparing to previous month]
- Recommendations: [suggested rule adjustments]

---

## Reference: Gate Dependency Map

```
Spec Approval Gate ⛔
        ↓
Pre-Implementation Scope Check ⛔
        ↓
Implementation (can run tests in parallel)
        ↓
Pre-Review Spec Alignment ⛔
        ↓
Breaking Change Detection (if applicable) ⛔
        ↓
Security Review (if applicable) ⛔
        ↓
Pre-PR Readiness Gate ⛔
        ↓
PR Merge
```

All gates are **non-blocking** to next gate except where marked ⛔ (hard stops).
