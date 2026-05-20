# Hook: post-spec.qa-analysis

## Purpose
Convert spec into actionable QA strategy and coverage expectations.

## Prompt Template

```text
HOOK: Post-Spec QA Analysis

Spec: specs/<feature-name>.md

Analyze and output:
1. Functional coverage targets
2. API contract validation targets
3. Regression-sensitive areas
4. Security-sensitive scenarios
5. Missing testable acceptance criteria

Output format:
- QA scope summary
- Missing coverage list
- Gate signal: READY_FOR_TEST_DESIGN or NEEDS_SPEC_UPDATES

Max output: 300 tokens.
```
