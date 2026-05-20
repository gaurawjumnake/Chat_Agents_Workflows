# Hook Execution Standard Format

All hooks follow this standard to ensure consistency across tools.

## Hook File Structure

Every hook prompt is a `.prompt.md` file with this structure:

```markdown
# [Hook Name]

## Purpose
[What this hook does in 1-2 sentences]

## Invocation Condition
[When to run this hook, which phase, what gate approval needed]

## Input Format (Strict)
[Exactly what inputs this hook needs, with examples]

## Execution Steps
[How to use this hook, step by step]

## Output Format
[Exactly what output to expect, with structure]

## Token Rules
[Input token limit, output token limit, context strategy]

## Gate Decision
[Which gate this hook produces, what are success criteria]

## On Failure
[What to do if hook output fails gate]

## Archive Location
[Where to save the output]
```

## Standard Hook Template

```markdown
# [Specific Hook Name]

## Purpose
One sentence describing what this hook validates/produces.

Example:
"Analyze spec to derive QA strategy and test matrix coverage gaps."

## Invocation Condition
- After: [prior phase/gate]
- Input: [what must be available]
- Status: [blocked by what, if anything]

Example:
"Run after REQUIREMENT_APPROVED gate.
Requires: Approved spec file exists.
Blocked by: If spec is incomplete."

## Input Format (Strict)
```
Format reference (copy-paste structure):
- Spec: specs/<feature>.md
- Prior analysis: [file path]
- Changed modules: [list]
- Framework path: [path]
```

Max input tokens: [number]
Example:
```
Spec: specs/payment-retry.md
Prior requirement summary: qa-memory/specs/payment-retry-qa.md
Changed modules:
  - src/payment/retry-strategy.ts
  - src/database/events.ts
Test framework: test/
```
```

## Execution Steps
Step-by-step instructions:
1. [Load inputs]
2. [Analyze]
3. [Produce output]
4. [Verify output]

## Output Format
Structured output template:

```
[Output heading 1]
[List / Table / Narrative]

[Output heading 2]
[List / Table / Narrative]

[Gate Decision]
[PASS / FAIL / CONDITIONAL]
```

Max output tokens: [number]

Example structure:
```
## QA Coverage Analysis

| Coverage Area | Status | Gaps |
|---|---|---|
| Functional | COMPLETE | None |
| API | PARTIAL | 3 scenarios missing |
| Regression | READY | Baseline known |

## Test Scenarios
1. [Positive case]
2. [Negative case]
3. [Edge case]

## Gate: SPEC_QA_APPROVED
Status: PASS (all coverage areas addressed)
```

## Token Rules
- Input budget: [X tokens]
- Output budget: [Y tokens]
- Context strategy: [which files to load]

Example:
```
Input budget: 700 tokens
  - Spec reference: 100 tokens
  - Requirement summary: 100 tokens
  - Prior QA context: 200 tokens
  - Impact map: 200 tokens
  - New inputs: 100 tokens

Output budget: 300 tokens
  - QA strategy: 150 tokens
  - Test matrix: 100 tokens
  - Gate decision: 50 tokens

Load from qa-context/: test-strategy.md
Load from qa-memory/: prior-feature-specs.md
Skip: Full code files
```

## Gate Decision
Gate name: [GATE_NAME]

Success criteria (ALL must be true):
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

Output status: [APPROVED / REJECTED / CONDITIONAL]

Example:
```
Gate: SPEC_QA_APPROVED
Success criteria:
- [ ] All functional scenarios covered
- [ ] API contract scenarios complete
- [ ] Regression scope is clear
- [ ] Security concerns identified

If ALL pass: APPROVED (proceed to Phase 5)
If ANY fail: REJECTED (return to spec phase)
```

## On Failure
If gate status is REJECTED:
1. [Identify which criteria failed]
2. [Go back to which phase]
3. [What to fix]
4. [Re-run steps]

Example:
```
If gate is REJECTED:

1. Check which coverage area failed (from output)
2. Return to spec phase with gap list
3. Update spec with missing requirements
4. Re-run this hook after spec is updated
```

## Archive Location
Save hook output to:
`qa-memory/[category]/[feature]-[hook-name].md`

Example:
```
Output file: qa-memory/specs/payment-retry-qa-analysis.md
Approval file: qa-memory/approvals/payment-retry-spec-qa-approved.md (if gated)
```

## Related Hooks
- [Hook A] runs before this
- [Hook B] runs after this
- [Hook C] runs in parallel

## Cross-Tool Compatibility
- ✅ Works with: Copilot, Claude, Cursor, Cline, Continue
- ✅ Uses: Standard markdown paths, no IDE-specific references
- ✅ Format: Plain text prompts (no JSON, no tool-specific syntax)
```

## Hook File Naming Convention

```
hooks/[phase]/[purpose].prompt.md

Examples:
- hooks/spec/post-spec.qa-analysis.prompt.md
- hooks/test/post-implementation.generate-tests.prompt.md
- hooks/security/pre-release.security-validation.prompt.md
- hooks/release/release-readiness-review.prompt.md
```

## Hook Execution Order Rules

Hooks must respect dependencies:

```
Pre-task hooks (context loading)
    ↓
Phase-specific hooks (in phase order)
    ↓
Parallel hooks (can run simultaneously)
    ↓
Post-phase hooks (archive and gate)
```

Example sequence for Phases 3-4:

```
1. Load context (pre-task)
2. Run post-spec.qa-analysis.prompt.md (Phase 3)
3. Run post-spec.risk-analysis.prompt.md (Phase 4, parallel with 2)
4. Collect outputs
5. Gate approval (both must pass)
6. Archive findings
7. Proceed to Phase 5
```

## Token Optimization in Hooks

Every hook should:
1. ✅ Load spec by reference (not full text)
2. ✅ Load changed modules only (diffs)
3. ✅ Reference qa-context files (not embed)
4. ✅ Reference qa-memory findings (not rebuild)
5. ❌ Never dump full repo
6. ❌ Never rebuild known context
7. ❌ Never ignore token budgets

## Testing Hook Quality

Before using a hook:

- [ ] Input format is clear
- [ ] Output format is machine-readable
- [ ] Token budget is realistic
- [ ] Gate criteria are testable
- [ ] On-failure path is clear
- [ ] Archive location is specified
- [ ] No IDE-specific assumptions
- [ ] No hardcoded file paths
- [ ] Works with multiple tools
