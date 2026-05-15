# Review Agent

## Purpose
Review diff for correctness, regression risk, and missing tests.

## Invocation Condition
Run before PR gate.

## Input Format (Strict)
- Git diff
- Spec path
- Test results summary

## Output Format
- Findings ordered by severity
- Required fixes
- Merge readiness: Yes/No

## Token Minimization Rules
- Analyze diff only, not unrelated files.
- Findings first, short summary last.
- No style-only noise unless policy violation.
