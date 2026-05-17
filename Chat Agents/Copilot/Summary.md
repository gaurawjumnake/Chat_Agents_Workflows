### GitHub Copilot

**Type:** IDE Extension + Chat Agent  
**Model:** GPT-5.x, Claude Sonnet/Opus 4.6, Gemini 3 (multi-model; switch with `/model`)  
**Pricing:** Free (2,000 completions/mo) · Pro $10/mo · Business $19/user/mo · Enterprise $39/user/mo  
**Repo/Docs:** https://docs.github.com/en/copilot

The most widely adopted AI coding tool at 20M+ users and ~42% market share. Works in VS Code, JetBrains, Neovim, Visual Studio, and Xcode — the widest IDE support of any tool here. Agent Mode (2025) added MCP support and background task delegation. Copilot Workspace works directly from GitHub issues and PRs.

**When to use:**
- Your team is already on GitHub and wants the lowest-friction AI adoption
- You need multi-IDE coverage across a mixed-editor team
- You want multi-model access (switch between Claude, GPT, Gemini mid-session with `/model`)
- Budget is a constraint — $10/mo Pro is the best value paid tier in this list

**When NOT to use:**
- You need deep autonomous multi-file reasoning — Claude Code or Codex are stronger
- You want full BYOK and model transparency — Copilot locks you to its model menu
- You need air-gapped / on-prem deployment — use Tabnine

**Accuracy:** Strong across all task types. Multi-model picker means you can route complex tasks to Claude Opus or GPT-5-thinking without leaving Copilot.

**Latency:** Fast inline completions. Chat responses are model-dependent.

**Token efficiency:** Medium. Does not expose token usage. Context window varies by model selected.

**IDE support:** VS Code, JetBrains, Visual Studio, Neovim, Xcode, Azure Data Studio, GitHub.com

**MCP:** Yes — Agent Mode supports MCP server connections.

**Privacy:** Cloud. April 2026 update opted users into AI training by default — check and update your settings. Enterprise plan adds admin controls and data isolation.

**Limitations:**
- June 2026 pricing moved to AI Credits (usage-based) — costs less predictable for heavy users
- Multi-file editing less reliable than Cursor
- Agent Mode still less capable than Claude Code or Codex for complex autonomous tasks
- Training data opt-in (default on) requires manual opt-out

---