# OpenAI Codex CLI — Usage Examples

**Type:** Terminal / CLI Agent + IDE Extension
**Docs:** https://developers.openai.com/codex/cli

---

## Basic Usage

```bash
# Start interactive TUI session in your project
cd /your/project
codex

# Ask about the codebase
> Explain how the request validation works in this API

# Single task, no interaction
codex "Rename processData to processUserData across the entire repo"
```

Codex reads the repository, proposes changes, and asks for approval before writing anything.

---

## Code Review

```bash
# Review before committing
codex "Review my uncommitted changes. Flag correctness issues only. 
Return a bullet list under 6 items."

# Second-pass review with a separate agent
codex "Run a security review on src/auth/ and return findings only"

# Switch models for deeper reasoning
/model gpt-5.4-thinking
```

Use Agent mode in the IDE extension to see proposed changes as a diff before accepting.

---

## Agentic / Multi-file Task

```bash
# Full automation — use with caution
codex --full-auto "Run tests and fix all failing test cases"

# CI/CD pipeline usage
codex exec --full-auto "Update CHANGELOG with commits since last tag"

# Parallel subagents for complex tasks
codex "Split this task into parallel subtasks:
1. Add input validation to all API endpoints
2. Write tests for each validation rule"
```

Use `--full-auto` for non-interactive automation in CI pipelines. Always create a git checkpoint first.

---

## Tips

- Create a git checkpoint before any Codex task — it edits files and you may want to revert
- Use `codex --full-auto` only in CI or sandboxed environments; prefer interactive mode locally
- Use `/model` inside the TUI to switch between GPT-5.4, GPT-5.3-Codex, and other models
- In VS Code, you can run Codex, Claude Code, and Copilot simultaneously as separate agents — each bills through its own subscription
- Codex in JetBrains embeds in the AI chat panel alongside other JetBrains AI providers — switch agents from the agent picker without changing tools
