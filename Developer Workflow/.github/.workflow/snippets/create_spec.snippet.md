# create_spec.snippet.md

## Use
Generate a spec from confirmed requirement and impact list.

## Prompt
```
Create a spec for this feature.
Input:
- Feature name: <name>
- Requirement: <short summary>
- Impacted files: <paths>
Output format:
Use sections:
- Summary
- Affected files
- API contracts
- Business logic
- Edge cases
- Test scenarios
- Non-goals
Target path: specs/<feature-name>.md
Keep concise and implementation-ready.
```
