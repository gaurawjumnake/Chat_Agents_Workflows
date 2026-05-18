# Claude Code — Usage Examples

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://docs.anthropic.com/en/docs/claude-code/getting-started

---

## Basic Usage

```bash
# Start an interactive session in your project
cd /your/project
claude

# Ask a question about your codebase
> How does the authentication flow work in this project?

# Make a targeted edit
> Fix the null pointer exception in src/auth/login.py line 47
```

Claude reads your project structure, identifies relevant files, and makes changes with a diff for review.

---

## Code Review

```bash
# Review a specific file or function
> Review the error handling in the payment processor. 
  Flag anything that could fail silently. Bullet points only.

# Review staged changes before commit
> Review my staged changes. Focus on correctness and edge cases.
  Keep feedback under 8 points.
```

Claude returns structured review comments. In VS Code / JetBrains, diffs appear inline in the IDE's diff viewer.

---

## Agentic / Multi-file Task

```bash
# Plan mode first — review before executing
> /plan Refactor the user service to use dependency injection
  across all files that reference it

# Then execute after reviewing the plan
> /execute

# Multi-step autonomous task
> Add comprehensive error handling to every API endpoint.
  Write tests for each error case. Commit when done.
```

Claude plans the approach, edits files across the repo, runs tests, and commits — pausing for your approval at each checkpoint.

---

## Tips

- Create a `CLAUDE.md` file at your repo root — Claude reads this at the start of every session for project-specific instructions (coding standards, architecture notes, what not to touch)
- Use `/plan` before any large task to review Claude's approach before it writes a single line of code
- Use `@filename` to reference a specific file in your prompt without pasting its contents
- For complex cross-file tasks, use Claude Opus (`/model claude-opus-4-6`) — it's slower but more accurate
- Claude Code uses 5.5x fewer tokens than Cursor on equivalent tasks — pair it with Copilot for inline completions
