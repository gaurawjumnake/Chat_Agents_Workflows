# Workflow Lifecycle & Routing

## Development Workflow Phases

The complete workflow consists of 10 phases, organized into 3 tracks:

```
TRACK 1: UNDERSTANDING (Phases 1-3)
├─ Phase 1: Requirement Analysis
├─ Phase 2: Impact Analysis
└─ Phase 3: Specification & Approval ⛔

TRACK 2: IMPLEMENTATION (Phases 4-7)
├─ Phase 4: Design
├─ Phase 5: Implementation
├─ Phase 6: Testing
└─ Phase 7: Debugging (if needed)

TRACK 3: FINALIZATION (Phases 8-10)
├─ Phase 8: Refactoring (optional)
├─ Phase 9: Review & Readiness ⛔
└─ Phase 10: Documentation & Deployment
```

## Phase Routing Logic

### Entry Point Routing Table

When a new requirement arrives, route based on:
1. **Does a spec already exist?**
2. **Work type?** (NEW_FEATURE | BUG_FIX | REFACTOR | REVIEW)
3. **Urgency?** (Normal | Hotfix | Emergency)

### Routing Decision Tree

```
Requirement Arrives
    ↓
├─ Is there an approved spec? 
│  ├─ YES → Skip to Phase 4 (Design)
│  └─ NO → Continue to Phase 1
│
├─ Work Type = NEW_FEATURE or MAJOR_REFACTOR?
│  ├─ YES → Full workflow (Phases 1-10)
│  └─ NO → Continue below
│
├─ Work Type = BUG_FIX?
│  ├─ YES → Phases 1-2 (quick), then 4-10
│  └─ NO → Continue below
│
├─ Work Type = REVIEW?
│  ├─ YES → Phase 9 (Review only)
│  └─ NO → Continue below
│
└─ Work Type = HOTFIX (Emergency)?
   ├─ YES → Phases 1-2 (minimal), then 5-9 (fast), skip Phase 10
   └─ NO → Normal workflow
```

### Workflow Shortcuts

#### New Feature (No Existing Spec)
```
Phase 1: Requirement Analysis (Requirement Agent)
    ↓ output: requirement summary
Phase 2: Impact Analysis (Impact Agent)
    ↓ output: affected files list
Phase 3: Specification (Spec Agent)
    ↓ [Spec Approval Gate] ⛔
Phase 4: Design (Design Agent) - optional
Phase 5: Implementation (Implementation Agent)
Phase 6: Testing (Test Agent)
Phase 7: Debugging (Debug Agent - if needed)
Phase 8: Refactoring - skip
Phase 9: Review (Review Agent) → Readiness Gate ⛔
Phase 10: Documentation
```
**Token Budget**: ~4000 tokens  
**Duration**: 2-4 hours  

#### Bug Fix (No Spec)
```
Phase 1: Requirement Analysis (Requirement Agent)
Phase 2: Impact Analysis (Impact Agent) - identify broken component
Phase 3: Specification (quick mini-spec) → Approval Gate ⛔
Phase 4: Design - skip
Phase 5: Implementation (Implementation Agent)
Phase 6: Testing (Test Agent - focus on regression)
Phase 7: Debugging (Debug Agent) - may loop back
Phase 8: Refactoring - skip
Phase 9: Review (Review Agent) → Readiness Gate ⛔
Phase 10: Documentation
```
**Token Budget**: ~2500 tokens  
**Duration**: 1-2 hours  

#### Feature (Spec Exists & Approved)
```
Phase 1-3: Skip (use existing spec)
Phase 4: Design (optional if spec is detailed)
Phase 5: Implementation (Implementation Agent)
Phase 6: Testing (Test Agent)
Phase 7: Debugging (if needed)
Phase 8-10: As normal
```
**Token Budget**: ~2000 tokens  
**Duration**: 1-3 hours  

#### Code Review
```
Phase 1-8: Skip (assume code exists)
Phase 9: Review (Review Agent)
    ├─ Pre-review spec check
    ├─ Detailed code review
    ├─ Breaking change detection
    └─ Readiness gate ⛔
Phase 10: Documentation (if changes to API/arch)
```
**Token Budget**: ~900 tokens  
**Duration**: 30 mins  

#### Hotfix (Emergency)
```
Phase 1: Requirement Analysis (fast)
Phase 2: Impact Analysis (fast - only critical paths)
Phase 3: Skip spec approval (log override)
Phase 4: Skip design
Phase 5: Implementation (Implementation Agent)
Phase 6: Testing (minimal regression tests)
Phase 7: Debugging (as needed)
Phase 8: Skip refactoring
Phase 9: Review (Review Agent)
Phase 10: Documentation (minimal - hotfix notes only)
```
**Token Budget**: ~1500 tokens  
**Duration**: 30-60 mins  
**Bypass Log**: Log all bypasses to `memory/incidents/hotfixes.md`

## Phase Details

### Phase 1: Requirement Analysis
**Agent**: Requirement Agent  
**Input**: Raw requirement text + constraints  
**Output**: Requirement summary + ambiguities/questions  
**Token Budget**: 500 tokens  
**Duration**: 10 minutes  

**Steps**:
1. Read requirement
2. Identify key terms + scope
3. List assumptions
4. Flag ambiguities
5. Ask clarifying questions (if needed)
6. Output requirement summary for user review

**Success Criteria**:
- Requirement summary is clear
- Scope is bounded (not too broad)
- No contradictions in requirement
- Assumptions documented

---

### Phase 2: Impact Analysis
**Agent**: Impact Agent  
**Input**: Approved requirement + codebase structure  
**Output**: List of affected files + modules  
**Token Budget**: 700 tokens  
**Duration**: 10 minutes  

**Steps**:
1. Parse requirement for entry points (APIs, UI elements, etc.)
2. Trace affected files (start from entry point)
3. Identify dependencies (modules that call this module)
4. Identify reverse dependencies (modules called by this module)
5. Output list of affected files with brief reason

**Success Criteria**:
- All affected files identified
- No unrelated files included
- Dependencies are traced accurately

**Memory**:
- Save impact analysis to `memory/decisions/DATE-impact.md`

---

### Phase 3: Specification
**Agent**: Spec Agent  
**Input**: Requirement + impact list  
**Output**: Specification document (specs/\<name\>.md)  
**Token Budget**: 900 tokens  
**Gate**: Spec Approval ⛔  
**Duration**: 30 minutes  

**Steps**:
1. Read requirement + impact list
2. Draft spec using spec-template.md
3. User approves spec (or requests changes)
4. Approved spec saved with APPROVED status

**Spec Contents** (must include):
- Summary (1 paragraph)
- Affected files list
- API contracts (inputs/outputs)
- Business logic (algorithms, rules)
- Edge cases
- Test scenarios
- Non-goals
- Breaking changes (if any)

**Success Criteria**:
- ✓ Spec passes approval checklist (see approval-rules.md)
- ✓ All approvers have signed off
- ✓ Spec is saved with APPROVED timestamp

**Failure Path**:
- Approver requests changes → Author revises spec → Re-submit for approval
- (Spec cannot proceed without approval - hard gate)

---

### Phase 4: Design (Optional)
**Agent**: Design Agent  
**Input**: Approved spec  
**Output**: Architecture notes + migration plan (if breaking change)  
**Token Budget**: 900 tokens  
**Duration**: 15 minutes  

**When to Skip**:
- Spec is simple (CRUD operation)
- Design is standard (no novel patterns)
- Similar feature exists (reuse existing design)

**When Required**:
- Breaking change (must plan migration)
- Novel architecture (no precedent)
- Cross-module changes (coordination needed)
- Performance-critical path

**Design Output** (if needed):
- Architecture diagram (ASCII or reference)
- Component interactions
- Data flow
- Migration path (if breaking change)

**Success Criteria**:
- Design aligns with spec
- Architecture decisions documented
- Migration plan is clear (if breaking change)

**Memory**:
- Save to `memory/decisions/DATE-design.md`

---

### Phase 5: Implementation
**Agent**: Implementation Agent  
**Input**: Spec + target files  
**Gate**: Pre-implementation scope check  
**Output**: Code diff  
**Token Budget**: 1200 tokens  
**Duration**: 30-60 minutes  

**Steps**:
1. Validate scope (target files match spec)
2. Load spec + affected files
3. Generate minimal code changes
4. Output diff/patch

**Minimal Changes Principle**:
- Change only what spec requires
- Do not refactor adjacent code
- Do not add features beyond spec
- Do not optimize unless spec requires it

**Success Criteria**:
- Code implements all spec requirements
- Code doesn't exceed spec scope
- Tests can pass with this code
- Token budget respected

**Memory**:
- Auto-save decision to `memory/decisions/DATE-implementation.md`

---

### Phase 6: Testing
**Agent**: Test Agent  
**Input**: Spec + implemented code  
**Output**: Test files  
**Token Budget**: 900 tokens  
**Duration**: 20 minutes  

**Test Coverage Target**: 80%+ for changed code  

**Test Types**:
- Unit tests (component-level)
- Integration tests (cross-module)
- Edge case tests (from spec)
- Regression tests (if bug fix)

**Success Criteria**:
- Tests pass (100% pass rate)
- Coverage >= 80%
- Edge cases covered
- No flaky tests

**Memory**:
- Auto-save test generation metadata

---

### Phase 7: Debugging (If Needed)
**Agent**: Debug Agent  
**Input**: Failing test + error log  
**Output**: Root cause + fix patch  
**Token Budget**: 900 tokens  
**Duration**: varies (5-30 minutes typical)  

**When Triggered**:
- Any test failure during Phase 6
- Can loop back to Phase 5 (fix code) or Phase 7 (debug more)

**Success Criteria**:
- Root cause identified
- Fix is minimal
- All tests pass
- No new failures introduced

**Memory**:
- Auto-save root cause analysis to `memory/incidents/DATE-bug.md`

---

### Phase 8: Refactoring (Optional)
**Agent**: Design Agent (can suggest refactoring)  
**Input**: Approved code  
**Output**: Refactoring recommendation or skipped  
**Token Budget**: 1000 tokens  
**Duration**: 10-20 minutes (if performed)  

**When to Perform**:
- Code has obvious duplication
- Similar pattern used 3+ times
- Performance can be improved without behavior change

**When to Skip**:
- Spec complete and tests pass
- No performance issues
- No maintainability concerns
- Time pressure (hotfix)

**Refactoring Rule**: **Only refactor code that's directly related to this change**  
(Avoid scope creep into other parts of codebase)

**Success Criteria**:
- Refactoring maintains all test passes
- No behavior changes
- Code is more maintainable

---

### Phase 9: Review & Readiness
**Agent**: Review Agent  
**Input**: Code diff + spec  
**Gate**: Pre-review spec check + Pre-PR readiness check  
**Output**: Review findings + approval  
**Token Budget**: 1500 tokens (divided across 3 checks)  
**Duration**: 20-30 minutes  

**Sub-phases**:

1. **Pre-Review Spec Check** (Review Agent)
   - Validates code aligns with spec
   - Checks all spec requirements implemented
   - Flags scope creep
   - **Gate**: Hard stop if spec misalignment

2. **Code Review** (Review Agent)
   - Risk-focused analysis
   - Security scan (if applicable)
   - Breaking change detection
   - Performance impact assessment
   - **Output**: Review findings list

3. **Pre-PR Readiness** (automated)
   - Tests passing? ✓
   - Coverage >= 80%? ✓
   - Linter/formatter passing? ✓
   - Documentation updated? ✓
   - Spec referenced in PR? ✓
   - **Gate**: Hard stop if any check fails

**Success Criteria**:
- ✓ Spec alignment approved
- ✓ Review findings all addressed
- ✓ All readiness checks pass
- ✓ PR approved by 1-2 reviewers

**Memory**:
- Auto-save findings to `memory/decisions/DATE-review.md`

---

### Phase 10: Documentation & Deployment
**Agent**: Documentation Agent  
**Input**: Approved PR + spec  
**Output**: Updated docs + changelog  
**Token Budget**: 700 tokens  
**Duration**: 15-20 minutes  

**Documentation Updates**:
- README (if API changes)
- Docstrings (for modified functions)
- Architecture notes (if design changed)
- Changelog entry (standardized format)
- Runbook (if deployment steps needed)
- Migration guide (if breaking change)

**Changelog Format**:
```
## [Type] <title>
<one-line summary>

**Spec**: [spec-path]
**Files**: [affected files]
**Breaking**: [Yes/No, if yes describe migration]
**Deployment**: [Any special steps?]
```

**Success Criteria**:
- Documentation is complete
- Changelog entry is accurate
- No typos or inconsistencies
- Ready for user communication

**Memory**:
- Auto-save changelog to `memory/workflows/changelog.md`

## Workflow Variations

### Parallel Tracks
Some phases can run in parallel:

```
Phase 5 (Implementation) & Phase 6 (Testing)
├─ Implementation Agent writes code
└─ Test Agent writes tests in parallel

Both complete → Merge → Run all tests

Benefit: Saves 10-15 minutes per workflow
```

### Context Preservation
Between phases, preserve:
- Spec path (cited in every phase)
- Affected files list (don't rescope)
- Test scenarios (reference throughout)
- Decisions made (cite in memory)

### Phase Skipping Rules
Can skip phases if:
- ✓ Requirement is trivial (1-2 line fix)
- ✓ Pattern is well-established (use existing template)
- ✓ Urgent hotfix (with bypass logging)
- ✓ Code review only (skip 1-8, do 9-10)

Cannot skip phases:
- ✗ Spec approval (always required)
- ✗ Testing (always required)
- ✗ Code review (always required)
- ✗ Pre-implementation scope check (always required)

## Routing Algorithm (Pseudocode)

```
Function Route(requirement, workType, urgency):
  spec = LookupSpec(requirement)
  
  if urgency == EMERGENCY:
    return EmergencyPath(requirement)
  
  if spec exists AND spec.approved:
    return SpecExists_Path(spec)
  
  if workType == CODE_REVIEW:
    return ReviewOnly_Path(requirement)
  
  if workType == BUG_FIX:
    return BugFix_Path(requirement)
  
  if workType == REFACTOR:
    return Refactor_Path(requirement)
  
  if workType == NEW_FEATURE:
    return NewFeature_Path(requirement)
  
  // Default
  return FullWorkflow_Path(requirement)
```

## Next Phase Trigger

A phase is complete when:
- ✓ Primary output is generated
- ✓ All gate checks pass (if applicable)
- ✓ Memory is updated
- ✓ No blocking issues remain

Then: **Automatically route to next phase**

## Routing Status
Current phase status and next step is saved in session memory for continuity across tool switches.
