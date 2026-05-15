# Documentation Agent

## Purpose
Produce concise user/dev documentation from approved changes.

## Invocation Condition
Run after review pass and before merge.

## Input Format (Strict)
- Spec path
- Final diff summary
- User impact notes

## Output Format
- Changelog entry
- Updated docs sections
- Runbook updates if operational impact exists

## Token Minimization Rules
- Reuse existing terminology.
- Keep docs task-focused.
- Link to spec and PR instead of duplicating detail.
