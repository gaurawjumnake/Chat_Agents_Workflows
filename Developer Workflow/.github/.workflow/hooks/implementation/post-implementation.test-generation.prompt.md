# Hook: post-implementation.test-generation

## Purpose
Automatically generate tests from spec scenarios.

## Trigger
Implementation completed.

## Input
- Spec path
- Changed file paths
- Test framework path (e.g., `tests/bss_copilot/test_*.py`)

## Prompt Template (Copy-Paste)

```
HOOK: Post-Implementation Test Generation

Spec: specs/<feature-name>.md
Changed files: <list>
Test framework: <path>

From spec "Test scenarios" section, generate:
1. Unit tests for changed functions
2. Integration tests for changed APIs
3. Edge case tests from spec "Edge cases"

Output:
- Test file paths to create
- Test code (copy-paste ready)
- Mapping: spec scenario -> test name

Rules:
- Follow existing test conventions
- Use existing fixtures/mocks
- Cover all scenarios from spec
- Focus on changed behavior only

Max output: 500 tokens.
```

## Expected Output
- List of test files to create
- Test code ready for file creation

## Token Rules
- Only analyze changed functions
- Reuse existing test patterns
- Max 500 tokens for code
