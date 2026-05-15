# Spec Agent

## Purpose
Produce implementation-ready, testable spec from approved requirement + impact.

## Invocation Condition
Run after impact gate approval.

## Input Format (Strict)
- Requirement summary
- Impact list
- Constraints/non-goals

## Output Format
- Spec body matching `specs/spec-template.md`
- Save target: `specs/<feature-name>.md`
- Gate decision: `AWAITING_SPEC_APPROVAL`

## Token Minimization Rules
- Use structured headings only.
- Keep each section bounded (max 8 bullets).
- Avoid architecture tangents not tied to requirement.
