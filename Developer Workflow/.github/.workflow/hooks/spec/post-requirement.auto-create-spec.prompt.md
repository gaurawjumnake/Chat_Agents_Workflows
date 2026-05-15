# Hook: post-requirement.auto-create-spec

## Purpose
Auto-draft a feature spec immediately after requirement and impact analysis.

## Trigger
Requirement summary and impact list are ready.

## Required Inputs
- Feature name
- Requirement summary (max 8 bullets)
- Impacted files/modules

## Optional Inputs
- Constraints
- Non-goals
- Existing related spec path

## Prompt Template (Copy-Paste)

```text
HOOK: Post-Requirement Auto Create Spec

Create and save a draft spec now.

Inputs:
- Feature: <feature-name>
- Requirement summary: <bullets>
- Impacted files/modules: <paths>
- Constraints: <optional>
- Non-goals: <optional>

Action:
1. Draft spec using the project spec format.
2. Save to: specs/<feature-name>.md
3. Keep sections concise and implementation-ready.

Required spec sections:
- Summary
- Affected files
- API contracts
- Business logic
- Edge cases
- Test scenarios
- Non-goals

Output format:
- Saved spec path
- 5-bullet summary of draft
- Missing info needed before approval (if any)

Do not implement code.
Next step: run hook `spec/approval-gate`.
```

## Expected Output
- Draft spec saved to `specs/<feature-name>.md`
- Short summary plus missing-info list

## Token Rules
- Use requirement + impact only
- Do not scan full repo
- Max output 220 tokens
