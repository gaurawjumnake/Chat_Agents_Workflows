# Cursor — Usage Examples

**Type:** AI-native IDE (VS Code fork)
**Docs:** https://cursor.com/en/docs

---

## Basic Usage

```
# Inline completion
Start typing — Cursor auto-completes as you write. Press Tab to accept.

# Inline edit (single file)
Select code → Cmd+K (Mac) / Ctrl+K (Windows/Linux)
> "Rewrite this to use async/await"

# Switch model for current task
Cmd+Shift+P → "Switch Model" → select Claude Opus / GPT-5 / Gemini
```

Cursor auto-completes and suggests as you type. Use `Cmd+K` for quick inline edits on a selection.

---

## Code Review

```
# Open Composer (multi-file chat)
Cmd+I → type your review request

> Review the changes I made in the last commit.
  Focus on the auth module. Return bullet points only.

# Reference a specific file
> @auth/login.py — is there a race condition in the token refresh logic?
```

Use `@filename` to reference any file. Composer reads it without you pasting the contents.

---

## Agentic / Multi-file Task

```
# Composer — multi-file task
Cmd+I → Agent mode ON

> Refactor the payment service to separate concerns:
  - Move validation to validators.py
  - Move DB calls to repository.py
  - Update all imports across the project

# Parallel agents (up to 8)
Open multiple Composer windows → run different tasks simultaneously
```

Agent mode plans the change, edits files across the repo, and shows a diff for review before applying.

---

## Tips

- Use `Cmd+I` (inline) for edits under 20 lines — it's faster and cheaper than Composer
- Use Composer (`Cmd+I` in Agent mode) for anything spanning multiple files
- Add a `.cursorrules` file at your repo root to set persistent instructions — Cursor reads it every session
- Reference files with `@filename` instead of pasting code — saves tokens and keeps prompts short
- Use `Cmd+Shift+J` to open Cursor Settings and switch the default model per task type (cheap model for completions, frontier model for Agent mode)
