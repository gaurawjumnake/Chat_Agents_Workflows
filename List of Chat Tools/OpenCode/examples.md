# OpenCode — Usage Examples

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://opencode.ai/docs

---

## Basic Usage

```bash
# Launch in your project folder
cd /your/project
opencode

# Ask about the codebase
> Explain how the database connection pool works here

# Switch between agents with Tab key
Tab  →  toggles between Build (full access) and Plan (read-only)

# Switch model
/models  →  select from available providers
```

Use **Plan mode** to explore and understand code without any changes. Switch to **Build mode** when ready to make edits.

---

## Code Review

```bash
# Review specific code
> Review the error handling in src/api/payments.py
  Flag anything that could silently swallow exceptions.
  Return bullet points only, under 8 items.

# Reference a file explicitly
> @src/auth/login.ts — is the token validation logic correct?
  Check for timing attacks.
```

OpenCode reads the file and returns a focused review. Use Plan mode for read-only review without risk of accidental edits.

---

## Agentic / Multi-file Task

```bash
# Multi-file refactor in Build mode
> Refactor the user module:
  - Extract validation into user_validators.py
  - Move DB queries into user_repository.py
  - Update all imports across the project

# Undo last change instantly
/undo

# GitHub integration — create a PR from a comment
# In a GitHub issue, comment:
/opencode implement the feature described in this issue
# OpenCode creates a branch, implements, and opens a PR
```

Use `/undo` to instantly revert any edit. Review the diff before accepting changes in Build mode.

---

## Tips

- Use **Plan mode** (Tab to switch) before any large task — it reads and analyses without touching files
- Use `/undo` immediately if OpenCode makes a change you didn't intend — it reverts the last edit
- Reference files with `@filename#L10-50` to point at specific line ranges
- For local/private code: authenticate with Ollama — code never leaves your machine
- Keyboard shortcuts in the IDE extension:
  - `Cmd+Esc` / `Ctrl+Esc` — open in split terminal
  - `Cmd+Option+K` / `Alt+Ctrl+K` — insert a `@file` reference from the current tab
