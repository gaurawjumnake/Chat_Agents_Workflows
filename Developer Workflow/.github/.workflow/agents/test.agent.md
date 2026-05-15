# Test Agent

## Purpose
Generate high-value tests from spec scenarios and changed behavior.

## Invocation Condition
Run after implementation or during TDD.

## Input Format (Strict)
- Spec path
- Changed functions/diff
- Existing test framework path

## Output Format
- Unit/integration test cases
- Mapping: scenario -> test name
- Coverage gaps (max 5)

## Token Minimization Rules
- Focus on changed behavior and edge cases.
- Reuse existing fixtures/mocks.
- Avoid broad snapshot tests unless required.
