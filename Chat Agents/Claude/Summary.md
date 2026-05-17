### Claude Code

**Type:** Terminal / CLI Agent  
**Model:** Claude Sonnet 4.6 (default) / Claude Opus 4.6  
**Pricing:** Bundled with Claude Pro ($20/mo) · Max 5× ($100/mo) · Max 20× ($200/mo). Real-world heavy use: $100–200/mo.  
**Repo/Docs:** https://docs.anthropic.com/en/docs/claude-code/overview

Terminal-native agent from Anthropic. Highest SWE-bench Verified score (80.8%). Works alongside any editor — it lives in the terminal, not inside an IDE. 1M token context window. Uses 5.5× fewer tokens than Cursor for equivalent tasks, making it cheaper at equivalent model quality. Supports hooks (event-driven automations), MCP natively, and skills/instructions files that persist across sessions.

**When to use:**
- Complex multi-file reasoning, architecture decisions, large refactors
- You prefer the terminal and want to keep your existing editor (Neovim, Emacs, VS Code, anything)
- You need the most accurate autonomous agent available today
- Long-context tasks (entire codebases, large diffs, architecture reviews)

**When NOT to use:**
- You want inline completions — Claude Code does not provide autocomplete (pair it with Copilot for that)
- You want a GUI — terminal only (though IDE extensions exist)
- Budget is tight — real heavy use costs $100–200/mo

**Accuracy:** Best-in-class. 80.8% SWE-bench Verified. Strongest reasoning depth of any tool here.

**Latency:** Fast on task execution. No inline completion (terminal interaction model).

**Token efficiency:** Very High. 1M token context; aggressive context compaction; graph tool integration cuts tokens further.

**IDE support:** Any editor via terminal. VS Code and JetBrains extensions available. Claude.ai web chat also available.

**MCP:** Yes — native, full support. MCP servers configured in `.mcp.json`.

**Privacy:** Cloud (Anthropic). Code sent to Anthropic API. Zero data retention on API tier. Pro plan code not used for training.

**Limitations:**
- No inline autocomplete — must pair with another tool for that
- Terminal UX; not beginner-friendly
- Rate limits apply even on Max plan ($200/mo) for automated workflows
- Locked to Anthropic models — no BYOK

---