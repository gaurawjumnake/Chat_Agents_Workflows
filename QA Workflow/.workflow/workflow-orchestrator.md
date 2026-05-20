# Workflow Orchestrator (Master Control)

**This is the single source of truth for QA workflow execution.**

Use this to understand:
- What phase to run
- What inputs are needed
- What outputs to expect
- What gates to enforce
- What to do on failure
- Where to store findings

---

## Quick Start

1. **New feature?** Start at Phase 1
2. **Execution phase?** Start at Phase 5
3. **Release?** Start at Phase 8
4. **Healing needed?** Jump to Phase 9
5. **Confused?** Read "Phase Context" below

---

## Global Execution Rules

### Rule 1: Always Use Specs
Every phase references the spec by path.
```
Spec: specs/<feature-name>.md
```
Never repeat requirement text in prompts.

### Rule 2: Load Context Selectively
Each phase loads ONLY its required files:
- Spec reference (metadata)
- Relevant qa-context file
- Relevant qa-memory note (if available)
- Changed modules/diffs (not full repo)

### Rule 3: Respect Token Budgets
Each phase has strict input/output token limits.
See `instructions/token-optimization/token-budgets.md`

### Rule 4: Enforce Hard Gates
Cannot proceed without gate approval.
See `instructions/core/approval-rules.md`

### Rule 5: Archive Findings
Every major decision goes to `qa-memory/`

---

## Phase Context and Execution

### PHASE 1: Requirement QA

**When**: Feature starts (requirement available)
**Who**: Requirement QA Agent
**Input**: Requirement note + acceptance criteria
**Output**: Testable requirement summary + ambiguities
**Max Tokens**: Input 300, Output 200

**Execution**:
```
Run: agents/requirement-qa.agent.md

Input:
- Requirement note (plain text)
- Acceptance criteria list
- Optional: similar prior specs

Output format:
- Summary of testable requirement
- Ambiguities and gaps
- Questions for stakeholder

Gate: REQUIREMENT_APPROVED
- Requirement is testable ✓
- Acceptance criteria clear ✓
- No major ambiguities ✓
```

**On Gate Failure**:
- Clarify requirement with stakeholder
- Update requirement note
- Re-run Phase 1

**Archive**: qa-memory/specs/[feature]-requirement-summary.md

---

### PHASE 2: QA Impact Analysis

**When**: After Phase 1 approved
**Who**: QA Impact Agent
**Input**: Requirement summary + entry points
**Output**: Affected scope + risk hotspots
**Max Tokens**: Input 400, Output 150

**Execution**:
```
Run: agents/qa-impact.agent.md

Input:
- Requirement summary (from Phase 1)
- Entry points (modules/APIs affected)
- Optional: diff if available

Output format:
- List affected modules
- List affected test suites
- Risk hotspots with confidence
- Ready for impact mapping

Gate: IMPACT_MAPPED
- Modules clearly identified ✓
- Test scope understood ✓
- Risk areas acknowledged ✓
```

**On Gate Failure**:
- Expand requirement clarification
- Run Phase 2 again

**Archive**: qa-memory/regression/[feature]-impact-map.md

---

### PHASE 3: Spec QA Analysis

**When**: After Phase 1-2 approved + spec is ready
**Who**: Functional Test Agent (or spec analysis hook)
**Input**: Approved spec + requirement summary
**Output**: QA strategy + test matrix gaps
**Max Tokens**: Input 700, Output 300

**Execution**:
```
Run: hooks/spec/post-spec.qa-analysis.prompt.md

Input:
- Spec: specs/<feature>.md
- Requirement summary (from Phase 1)
- Prior QA context: qa-context/test-strategy.md

Output format:
- QA strategy coverage (functional/API/regression/security)
- Functional test scenarios (positive/negative/edge)
- API test scenarios (if applicable)
- Missing coverage areas

Gate: SPEC_QA_APPROVED
- QA coverage strategy complete ✓
- Test matrix reviewed ✓
- Missing coverage acknowledged ✓
```

**On Gate Failure**:
- Update spec with missing requirements
- Rerun Phase 3

**Archive**: qa-memory/specs/[feature]-qa-analysis.md

---

### PHASE 4: Risk & Threat Analysis

**When**: In parallel with Phase 3, or right after
**Who**: Risk Agent + Threat Model Agent
**Input**: Spec + impact map + security context
**Output**: Risk register + threat model updates
**Max Tokens**: Input 700, Output 300

**Execution**:
```
Run: hooks/spec/post-spec.risk-analysis.prompt.md

Input:
- Spec: specs/<feature>.md
- Impact map (from Phase 2)
- Security model: qa-context/security-model.md
- Prior risks: qa-memory/security/[related].md

Output format:
- Risk register (category, severity, mitigation)
- Threat model updates (new threats identified)
- Security coverage (auth, RBAC, abuse, deps)
- Unacceptable risks (if any)

Gate: RISK_APPROVED
- Risk register complete ✓
- Unacceptable risks identified ✓
- Mitigations planned ✓
```

**On Gate Failure**:
- Identify unacceptable risks
- Plan mitigations
- Rerun Phase 4

**Archive**: qa-memory/security/[feature]-threat-model.md

---

### PHASE 5: Test Design & Execution

**When**: After Phase 3-4 approved + implementation ready
**Who**: Functional Test Agent + API Test Agent
**Input**: Spec + changed modules/diff + test framework
**Output**: Test results + root cause analysis
**Max Tokens**: Input 800, Output 500

**Execution**:
```
Run (in sequence):
1. hooks/test/post-implementation.generate-tests.prompt.md
2. Execute tests
3. Archive results

Input:
- Spec: specs/<feature>.md
- Changed modules: [list]
- Diff (if available)
- Test framework path
- Existing test patterns: qa-memory/patterns/

Output format:
- Generated test cases (functional/API/automation)
- Test execution results
- Failed tests (if any)
- Root cause analysis (for failures)

Gate: TEST_RESULTS_READY
- Critical scenarios pass ✓
- Failures triaged ✓
- Regression ready to run ✓
```

**On Gate Failure**:
- Triage failures (bug or test issue?)
- Go to Phase 6 (Test Execution & Debug) if failures need analysis

**Archive**: qa-memory/bugs/[bug-id].md (for found bugs)

---

### PHASE 6: Test Execution & Debug

**When**: If Phase 5 has failures or flakiness
**Who**: Bug Analysis Agent + Healing Agent
**Input**: Failed tests + logs + metrics
**Output**: Root cause + bug evidence + healing proposals
**Max Tokens**: Input 900, Output 400

**Execution**:
```
Run (in sequence, if needed):
1. agents/bug-analysis.agent.md (if bug found)
2. hooks/healing/post-failure.healing-analysis.prompt.md (if flaky)

Input:
- Failed test output (critical lines only)
- Relevant logs (last 50 lines)
- Environment info
- Flakiness metrics (if available)

Output format:
- Root cause identified
- Bug evidence (log snippet + metric)
- Healing proposal (if flaky)
- Next action (fix bug / apply healing / escalate)

Gate: FAILURES_TRIAGED
- Root causes identified ✓
- Bug tickets filed (if bugs) ✓
- Healing ready for approval (if flaky) ✓
```

**On Gate Failure**:
- Insufficient root cause evidence → Rerun with more logs
- Healing proposal unsafe → File bug ticket instead

**Archive**: 
- qa-memory/bugs/[bug-id].md
- qa-memory/healing/[healing-id].md (if healing proposed)

---

### PHASE 7: Regression Scope

**When**: In parallel with Phase 5-6, after impact mapped
**Who**: Regression Agent
**Input**: Changed modules + regression map
**Output**: Minimal regression subset + baseline
**Max Tokens**: Input 600, Output 250

**Execution**:
```
Run: hooks/regression/regression-scope-update.prompt.md

Input:
- Changed modules: [list from Phase 2]
- Regression baseline: qa-context/regression-map.md
- Prior failures: qa-memory/regression/

Output format:
- Affected test suites (from regression map)
- Regression test subset (minimal, high-confidence)
- Baseline comparison (if applicable)
- Recommended run time (for planning)

Gate: REGRESSION_SCOPED
- Scope is minimal ✓
- High-confidence tests selected ✓
- Ready to run ✓
```

**On Gate Failure**:
- Scope is too large → Reduce to critical tests only
- Baseline unclear → Update qa-context/regression-map.md first

**Archive**: qa-memory/regression/[feature]-scope.md

---

### PHASE 8: Security Validation

**When**: Before release, after Phase 3-4-7
**Who**: Security Agent + Threat Model Agent
**Input**: Spec + changed code + security model
**Output**: Security verdict + required actions
**Max Tokens**: Input 900, Output 350

**Execution**:
```
Run: hooks/security/pre-release.security-validation.prompt.md

Input:
- Spec: specs/<feature>.md
- Changed modules (diffs)
- Security model: qa-context/security-model.md
- Prior findings: qa-memory/security/

Output format:
- Auth/RBAC validation (pass/fail)
- Dependency security scan (pass/fail + details)
- Secret exposure check (pass/fail)
- Abuse path analysis (covered/not covered)
- Verdict: PASS / FAIL / CONDITIONAL

Gate: SECURITY_PASS
- Auth/RBAC validated ✓
- Dependencies secured ✓
- No secrets exposed ✓
- Abuse paths mitigated ✓
```

**On Gate Failure**:
- Critical vulnerabilities → File security ticket, block release
- Missing coverage → Update spec, rerun Phase 8
- Conditional pass → Remediate and rerun before release

**Archive**: qa-memory/security/[feature]-validation.md

---

### PHASE 9: Healing Analysis

**When**: Only if Phase 6 identified flaky tests (OPTIONAL)
**Who**: Healing Agent
**Input**: Flaky test telemetry + root cause
**Output**: Healing proposal + guardrails + approval gate
**Max Tokens**: Input 700, Output 300

**Execution**:
```
Run: hooks/healing/post-failure.healing-analysis.prompt.md

Input:
- Flaky test: [test path]
- Root cause evidence (logs + metrics)
- Prior healing attempts: qa-memory/healing/
- Healing guardrails (what NOT to do)

Output format:
- Healing proposal (specific action)
- Safe reversal plan (in case healing fails)
- Test coverage for healing
- Approval requirements

Gate: HEALING_APPROVED
- Root cause is clear ✓
- Healing is safe and reversible ✓
- Test guardrails in place ✓
- Human approval obtained ✓
```

**On Gate Failure**:
- Healing is unsafe → File bug ticket instead
- Root cause unclear → Collect more evidence, rerun

**Apply Healing**: Only after approval gate is signed
```
Apply healing action (from Phase 9 approval)
Rerun test
Record: qa-memory/healing/[feature]-applied.md
```

**Archive**: qa-memory/healing/[feature]-analysis.md (includes approval)

---

### PHASE 10: Release Validation

**When**: Ready to release (all prior phases complete)
**Who**: Release Validation Agent
**Input**: Smoke test results + risk register
**Output**: Go/No-Go decision
**Max Tokens**: Input 700, Output 200

**Execution**:
```
Run (in sequence):
1. hooks/release/pre-release.smoke-validation.prompt.md
2. hooks/release/release-readiness-review.prompt.md

Input:
- Smoke test suite results (pass/fail)
- Risk register: qa-memory/security/[feature]-validation.md
- All prior gate approvals
- Environmental readiness

Output format:
- Smoke validation: PASS / FAIL
- Release readiness: READY / NOT_READY
- Final decision: GO / NO-GO
- Blockers (if any)

Gate: RELEASE_APPROVED
- All prior gates passed ✓
- Smoke validation passed ✓
- No critical unresolved risks ✓
- Release readiness confirmed ✓
```

**On Gate Failure**:
- Smoke test failed → Go back to Phase 5, fix failures
- Unresolved risk → Go back to Phase 4, mitigate
- Not ready → Delay release, resolve blockers

**Archive**: qa-memory/approvals/[feature]-release-go-no-go.md

---

## Phase Dependency Graph

```
REQUIREMENT_QA (Phase 1)
      ↓
IMPACT_MAPPED (Phase 2)
      ↓
  ┌─────────────────┐
  ↓                 ↓
SPEC_QA_APPROVED   RISK_APPROVED
(Phase 3)          (Phase 4)
  ├─────────────────┤
  ↓
TEST_RESULTS_READY (Phase 5-6)
  ├─────────────────┐
  ↓                 ↓
REGRESSION_SCOPED  SECURITY_PASS
(Phase 7)          (Phase 8)
  ├─────────────────┤
  ↓
HEALING_APPROVED (Phase 9, optional)
  ↓
RELEASE_APPROVED (Phase 10)
```

---

## Failure Recovery Paths

| If This Fails | Go Back To | Re-Run | Then Proceed To |
|---|---|---|---|
| REQUIREMENT_APPROVED | Requirement phase | Phase 1 | Phase 2 |
| IMPACT_MAPPED | Requirement review | Phase 2 | Phase 3-4 |
| SPEC_QA_APPROVED | Spec review | Phase 3 | Phase 5 |
| RISK_APPROVED | Threat modeling | Phase 4 | Phase 8 |
| TEST_RESULTS_READY | Implementation | Phase 5-6 | Phase 7-8 |
| REGRESSION_SCOPED | Regression baseline | Phase 7 | Phase 10 |
| SECURITY_PASS | Threat mitigation | Phase 8 | Phase 10 |
| HEALING_APPROVED | Root cause analysis | Phase 6 | (File bug instead) |
| RELEASE_APPROVED | Blocker resolution | Phase 5-8 | Deploy |

---

## Minimal Input Examples

### Example 1: New Feature Validation
```
PHASE 1 (Requirement QA):
Input:
  Requirement: Payment retry API to handle transient failures
  AC:
    - Retry up to 3 times
    - Exponential backoff with jitter
    - Log all retry attempts

Output:
  Testable? YES
  Ambiguity: "jitter range?" (clarify)
  Blocking: NO
  Ready for Phase 2? YES
```

### Example 2: Mid-Project Issue
```
PHASE 6 (Test Execution & Debug):
Input:
  Failed test: test_payment_retry_timeout
  Error: timeout after 5s (expected 10s)
  Logs: [last 20 lines]
  
Output:
  Root cause: Backoff calculation includes extra 5s sleep
  Bug ticket: retry-strategy#123
  Healing: NO (it's a bug)
```

### Example 3: Before Release
```
PHASE 10 (Release Validation):
Input:
  Smoke tests: 45/45 PASS
  Risk register: 1 LOW, 0 MEDIUM/HIGH
  Security: PASS

Output:
  Decision: GO (ready to release)
  Approval: [signed]
```

---

## Tool Integration (Tool-Agnostic)

This orchestrator works with all coding agents:
- GitHub Copilot
- Claude Code
- Cursor
- Cline
- Continue
- OpenCode
- Windsurf

Each tool uses the same:
- Phase definitions
- Input/output format
- Token budgets
- Gate rules
- Memory paths
- Spec references

No IDE-specific logic in phases.

---

## When in Doubt

1. **What phase am I in?** → Read phase name + description
2. **What should I do next?** → Follow "Execution" section
3. **What are my inputs?** → Read "Input:" subsection
4. **How many tokens?** → Read "Max Tokens:" subsection
5. **What's the output?** → Read "Output format:" subsection
6. **Can I proceed?** → Check "Gate:" requirements
7. **What do I save?** → Read "Archive:" subsection
8. **What if I fail?** → Read "On Gate Failure:" subsection
9. **Need token rules?** → See instructions/token-optimization/
10. **Need approval rules?** → See instructions/core/approval-rules.md

---

**Last Updated**: 2026-05-20
**Version**: 1.0
**Status**: Master Control Document
