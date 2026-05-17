# GitHub Copilot — Usage Examples

**Type:** IDE Extension + Terminal CLI
**Docs:** https://docs.github.com/en/copilot

---

## Basic Usage

```
# Inline suggestion
Start typing a function — Copilot completes it. Press Tab to accept.

# Switch model mid-session (Agent mode)
/model claude-opus-4-6
/model gpt-5
/model gemini-3-pro
```

Inline suggestions work as you type. Chat works in the sidebar panel or inline with `Cmd+I` / `Ctrl+I`.

---

## Code Review

```
# In Copilot Chat
Review this function for edge cases and security issues.
Focus only on the validation logic. Return bullet points only.

# From a PR diff (GitHub.com)
@copilot review this PR — summarise the changes and flag anything risky
```

Copilot reads the diff and returns structured review comments. In Agent mode it can post comments directly to the PR.

---

## Agentic / Multi-file Task

```
# Agent mode (Copilot Chat with Agent mode enabled)
Refactor the authentication module to support OAuth2.
Update all affected files. Show me a plan before making changes.

# Background agent (prefix with &)
& Add unit tests for every public method in src/payments/
```

Agent mode plans the approach, writes code, and verifies across your project. Background agents (`&` prefix) run async while you work.

---

## Tips

- Run `/init` at the start of a new project to generate custom instructions from your codebase
- Use `@workspace` to give Copilot context about your full project, not just the open file
- Use `/model` to switch between Claude, GPT, and Gemini per task without leaving the chat
- Create `.github/copilot-instructions.md` to set persistent rules (coding style, constraints, what to avoid) — Copilot reads this automatically every session
- Use inline chat (`Cmd+I`) for small edits; open the sidebar panel for multi-step tasks
