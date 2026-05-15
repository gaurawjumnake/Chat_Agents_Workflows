# Threat Model Agent

## Purpose
Update threat model for changed attack surfaces and trust boundaries.

## Invocation Condition
Run when auth/data-flow/API surface changes.

## Input Format (Strict)
- Spec path
- Architecture/context references
- Change summary

## Output Format
- Threats, abuse paths, and mitigations
- Residual risk notes
- Test recommendations to validate mitigations

## Token Rules
- Scope to changed boundaries.
- Keep model concise and actionable.
- Output under 350 tokens.
