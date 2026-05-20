# Snippet Building Blocks Standard

All QA snippets follow this standard for reusability and composition.

## Snippet File Structure

Every snippet is a `.snippet.md` file with this structure:

```markdown
# [Snippet Name]

## Purpose
[What this snippet does, when to use it]

## When to Use
[Specific conditions, which phase(s)]

## Input Format
[Exactly what inputs needed]

## Prompt Template
[Copy-paste prompt text]

## Output Format
[Expected output structure]

## Token Rules
[Input/output budgets]

## Common Variations
[How to adapt this snippet]

## Composition Rules
[How to combine with other snippets]

## Examples
[Real usage examples]
```

## Standard Snippet Template

```markdown
# [Specific Snippet Name]

## Purpose
Short description of what this snippet helps you do.

Example: "Generate functional test cases from spec behavior."

## When to Use
- Phase: [which phase(s)]
- Condition: [when applicable]
- Replaces: [what it replaces]
- Prereq: [what must be done first]

Example:
"Phase 5 (Test Design & Execution)
After spec QA analysis is approved.
Generates: Functional test scenarios
Prerequisite: Spec and implementation available"

## Input Format
What you need to provide:

```
Input checklist:
[ ] Spec path
[ ] Changed modules/diff
[ ] Test framework location
[ ] Existing test pattern examples
[ ] Edge cases (optional)
```

Template:
```
Spec: specs/<feature>.md
Changed modules:
  - <module-path-1>
  - <module-path-2>
Test framework: <path>
Edge cases:
  - [edge case 1]
  - [edge case 2]
```

Max input tokens: [number]

## Prompt Template

This is copy-paste ready (fill in brackets):

```
Generate functional test cases for this change.

Spec: [specs/feature.md]
Changed behavior:
[Copy from "Changed modules" above]

Requirements:
1. Generate positive/negative/edge cases
2. Reuse existing test patterns
3. Include scenario descriptions
4. Keep tests deterministic and isolated
5. Focus on changed behavior only

Output:
1. List of test scenarios
2. Scenario -> implementation mapping
3. Coverage gaps (if any)
4. Estimated test time
```

## Output Format

You should expect structured output:

```
## Functional Test Scenarios

### Scenario 1: [Name]
- Given: [precondition]
- When: [action]
- Then: [expected result]
- Test file: [test path]

### Scenario 2: [Name]
...

## Coverage Summary
| Area | Scenarios | Status |
|---|---|---|
| Positive cases | 2 | COMPLETE |
| Negative cases | 3 | COMPLETE |
| Edge cases | 2 | PARTIAL |

## Coverage Gaps
- [gap 1]
- [gap 2]

## Estimated Execution Time
~12 minutes
```

Max output tokens: [number]

## Token Rules

Budget:
- Input: [X tokens]
  - Spec summary: [A tokens]
  - Changed modules: [B tokens]
  - Test patterns: [C tokens]
  - New data: [D tokens]

- Output: [Y tokens]
  - Test scenarios: [A tokens]
  - Coverage summary: [B tokens]
  - Implementation notes: [C tokens]

Context strategy:
- Load: specs/, qa-memory/patterns/
- Reference (don't load): full spec text
- Skip: entire repository, all test files

## Common Variations

### Variation 1: Minimal Tests
Use when: Quick validation needed, high confidence change

```
Skip: Edge cases
Focus: Critical path only
Output: 2-3 test cases instead of 5+
```

### Variation 2: Extended Coverage
Use when: High-risk area, new functionality

```
Add: Security test scenarios
Add: Performance test scenarios
Add: Backward compatibility tests
Output: 10+ test cases
```

### Variation 3: Regression-Only
Use when: Change only affects existing code path

```
Skip: New scenario generation
Use: Regression test list from qa-context/regression-map.md
Output: Scope of existing tests to rerun
```

## Composition Rules

Snippets are composable. Chain them like:

```
Step 1: Run test_generation.snippet
  Output → Step 1 findings

Step 2: Run edge_case_generation.snippet
  Input: Step 1 findings + spec
  Output → Step 2 findings

Step 3: Use findings for actual test implementation
  Input: Step 1 + Step 2 findings
  Output → Test code
```

Rules for composition:
- Each snippet produces output suitable for next snippet
- Output format must match next snippet's input format
- Save intermediate outputs to qa-memory/
- Respect token budgets in each snippet independently

## Examples

### Example 1: Simple Feature
```
Input:
Spec: specs/email-validation.md
Changed modules:
  - src/validators/email.ts
Test framework: test/

Snippet: test_generation.snippet

Output:
Test scenarios:
1. Valid email formats
2. Invalid formats
3. Edge cases (very long, special chars)

Coverage: COMPLETE
```

### Example 2: Complex API Change
```
Input:
Spec: specs/payment-api-v2.md
Changed modules:
  - src/payment/endpoints.ts
  - src/payment/models.ts
Test framework: test/

Snippet: api_contract_test.snippet

Output:
API test scenarios:
1. Request validation
2. Response contract (schema)
3. Error cases
4. Backward compatibility

Coverage: 4 areas covered
```

### Example 3: Regression Analysis
```
Input:
Changed modules:
  - src/auth/token-refresh.ts
Regression baseline: qa-context/regression-map.md#auth

Snippet: regression_analysis.snippet

Output:
Affected test suites:
- test/auth/token-refresh.spec
- test/integration/login-flow.spec

Scope: 2 suites, ~15 tests
```

## Quality Checklist

Before using a snippet:

- [ ] Purpose is clear
- [ ] Input format is specific (not vague)
- [ ] Prompt is copy-paste ready
- [ ] Output format is structured
- [ ] Token budget is realistic
- [ ] Variations are documented
- [ ] Composition rules are clear
- [ ] Examples are concrete
- [ ] No IDE-specific assumptions
- [ ] Works standalone and in chains

## Snippet Catalog

Location: `snippets/qa/`

Available snippets:
- `test_generation.snippet.md` - Functional test design
- `api_contract_test.snippet.md` - API test design
- `edge_case_generation.snippet.md` - Edge case discovery
- `regression_analysis.snippet.md` - Regression scope
- `security_validation.snippet.md` - Security checks
- `healing_analysis.snippet.md` - Flaky test analysis
- `release_validation.snippet.md` - Release readiness
- `bug_analysis.snippet.md` - Bug root cause
- `[add new snippet using this template]`

## When to Create New Snippet

Create a new snippet when:
1. Task is repeatable (used in multiple features)
2. Input/output format is clear
3. Scope is narrow (not a full workflow phase)
4. Can be independent of other snippets

Don't create snippet when:
- It's a one-time task
- It requires full workflow context
- Better as a hook (stateful, gated)
- Better as agent guidance (deep reasoning)
