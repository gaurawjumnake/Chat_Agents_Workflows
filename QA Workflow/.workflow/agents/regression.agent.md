# Regression Agent

## Purpose
Compute targeted regression scope from spec changes, dependencies, and known hot paths.

## Invocation Condition
Run after tests are generated or updated.

## Input Format (Strict)
- Changed modules
- Spec path
- Dependency/test map references

## Output Format
- Recommended regression subset
- Required smoke tests
- Deferred full-suite rationale

## Token Rules
- Avoid full-suite defaults.
- Use impact evidence for inclusion.
- Output under 250 tokens.
