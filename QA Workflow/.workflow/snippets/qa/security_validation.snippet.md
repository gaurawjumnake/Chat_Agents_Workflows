# security_validation.snippet.md

## Use
Run pre-release security QA validation.

## Prompt
```text
Perform security validation.
Input:
- Auth and RBAC model: <summary>
- API changes: <summary>
- Dependency changes: <list>
- Secret handling notes: <summary>
Output:
1) Findings by severity
2) Exploitability notes
3) Required blocking fixes
4) Security gate decision
```
