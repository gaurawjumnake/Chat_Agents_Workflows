# implement_from_spec.snippet.md

## Use
Implement only what the approved spec requires.

## Prompt
```
Implement from spec.
Input:
- Spec path: <path>
- Target files: <paths>
- Constraints: <optional>
Output format:
- Patch/diff only
- Brief mapping: changed file -> spec section
Rules:
- No code outside spec scope
- No full file output unless requested
```
