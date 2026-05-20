# QA Memory and Context Loading Strategy

## QA Context Layer (Stable Reference)

QA context is **write-once, read-often** stable strategy artifacts.

Location: `qa-context/`

| File | Content | Update Frequency | Usage |
|------|---------|------------------|-------|
| test-strategy.md | Scope pillars, entry/exit criteria | Once per project | Load for all test phases |
| security-model.md | Auth/RBAC/abuse/dependency checks | Quarterly | Load for security phase |
| regression-map.md | Affected modules by feature area | Per feature | Load for regression phase |
| api-test-matrix.md | API endpoint coverage | Quarterly | Load for API test phase |
| flaky-tests.md | Known flaky tests + root cause | Monthly | Load for test execution |
| environments.md | Test/staging/prod environment specs | Quarterly | Load for release phase |

### When to Load QA Context

Load context file when:
- Phase requires it (e.g., security validation → load security-model.md)
- Prior analysis references it
- Context helps avoid re-analysis

Don't load:
- All files at once (too many tokens)
- If current phase doesn't need it
- If file was just updated (use latest update)

### How to Reference QA Context

```
Test strategy: qa-context/test-strategy.md
Security model: qa-context/security-model.md
Regression baseline: qa-context/regression-map.md#payment-retry
API coverage: qa-context/api-test-matrix.md
Flaky tests: qa-context/flaky-tests.md
Environments: qa-context/environments.md#staging
```

## QA Memory Layer (Write-Often, Read-Selective)

QA memory stores **historical findings** organized by category.

Location: `qa-memory/`

| Category | Content | Retention | Query Pattern |
|----------|---------|-----------|---------------|
| bugs/ | Root causes, reproducibility steps | 12 months | By feature or component |
| security/ | Vulnerabilities, remediations | Permanent | By threat type |
| healing/ | Flaky test analyses, approvals | 6 months | By flaky test pattern |
| patterns/ | Reusable QA validation patterns | Permanent | By validation type |
| regression/ | Test scope history | 12 months | By release cycle |
| specs/ | QA notes linked to specs | Lifetime | By spec name |
| snippets/ | Snippet usage notes and variants | 6 months | By snippet name |

### When to ADD to QA Memory

Add note when:
- Bug root cause is novel or complex
- Security finding is important or recurring
- Healing analysis requires approval (MANDATORY)
- Regression scope needs historical context
- Pattern is reusable across features

Don't add:
- Transient failures (flake with no root cause)
- Standard debugging steps
- Duplicate findings (update existing note instead)

### When to READ from QA Memory

Read note when:
- Similar feature or bug arises
- Pattern matches prior healing/flaky test
- Need approval history for similar issue
- Security finding has related mitigations

Pattern for reading:

```
Similar prior analysis: qa-memory/bugs/payment-timeout.md

Current finding: [similar / different]
Reuse mitigation: [yes / no, new approach]
```

### Memory Note Structure (Template)

All memory notes use simple format:

```
# [Title]

## Feature / Component
[Which feature or component]

## Issue / Finding
[What was the problem or finding]

## Root Cause / Analysis
[Why it happened or deep analysis]

## Remediation / Approach
[How to fix or approach it]

## Approval (if needed)
- Approver: [role]
- Date: [date]
- Conditions: [if any]

## Related Links
- Spec: specs/...md
- Test: test/...spec.ts
- Prior analysis: qa-memory/...md
```

## Memory Update Rules

### Monthly Cleanup
- Archive resolved bugs (move to `qa-memory/archive/`)
- Remove duplicate entries (consolidate findings)
- Prune expired entries (> 12 months old)

### Before Major Release
- Verify all security findings are resolved
- Confirm all healing analyses have approval
- Update regression scope summary

### Incremental Updates
Never rebuild entire memory. Instead:

```
Prior note: qa-memory/bugs/payment-retry.md

Update (append):
## NEW FINDING [Date]
Additional root cause discovered: [specific]
Impact: [unchanged / expanded]
Remediation: [updated]
```

## Memory-Context Relationship

```
qa-context/regression-map.md
    ↓ (uses findings from)
qa-memory/regression/payment-retry-scope.md
    ↓ (references)
qa-memory/bugs/payment-timeout.md

qa-context/security-model.md
    ↓ (depends on)
qa-memory/security/auth-abuse-paths.md
    ↓ (validated in)
qa-memory/specs/payment-retry-spec-qa.md
```

## Memory Loading by Phase

| Phase | Load | Purpose |
|-------|------|---------|
| 1. Requirement QA | None (first phase) | Fresh analysis |
| 2. QA Impact | qa-memory/regression/ | Understand prior impacts |
| 3. Spec QA | qa-memory/specs/ | Validate spec coverage |
| 4. Risk Analysis | qa-memory/security/, qa-memory/bugs/ | Identify recurring risks |
| 5. Test Design | qa-memory/patterns/ | Reuse test patterns |
| 6. Test Execution | qa-memory/bugs/, flaky-tests.md | Avoid known issues |
| 7. Regression | qa-context/regression-map.md, qa-memory/regression/ | Scope efficiently |
| 8. Security | qa-context/security-model.md, qa-memory/security/ | Validate against model |
| 9. Healing | qa-memory/healing/ | Check prior approvals |
| 10. Release | qa-memory/approvals/ | Confirm all gates |

## Cross-Tool Compatibility

Memory paths use standard format (no IDE-specific paths):
- ✅ `qa-memory/bugs/bug-123.md`
- ✅ `qa-context/regression-map.md#payment`
- ❌ `.vscode/memory/bug-123.json`
- ❌ `C:\Users\...\qa-memory\bug-123.md`

All tools (Copilot, Claude, Cursor, Cline, etc.) use same paths.
