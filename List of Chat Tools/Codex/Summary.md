### OpenAI Codex

**Type:** Terminal / CLI + IDE Extension  
**Model:** GPT-5.x (locked to OpenAI)  
**Pricing:** CLI is open source (Apache-2.0). Usage requires ChatGPT Plus ($20/mo) or Pro ($200/mo). API: pay per token.  
**Repo/Docs:** https://github.com/openai/codex (CLI) · https://platform.openai.com/docs/guides/code

Re-emerged in 2025 as a serious agentic tool — not the legacy autocomplete model name. Deterministic on multi-step tasks: understands repo structure, makes coordinated changes, runs tests, and iterates without drifting. Cloud agent (Codex App) runs in OpenAI's sandboxed environment. CLI is open source and runs locally. Native GitHub integration (February 2026) lets it work directly on repos, issues, and PRs. Leads terminal-bench (240+ tok/s). JetBrains native integration available as first-class agent in AI chat.

**When to use:**
- You are already in the OpenAI ecosystem (ChatGPT Pro/Plus)
- You need fast, multi-step agentic execution with high follow-through
- You want native GitHub repo integration
- CLI/terminal-oriented workflows

**When NOT to use:**
- You want model flexibility — Codex is OpenAI-only, no BYOK, no local models
- You need local/private execution — cloud sandbox only
- You want the strongest reasoning — Claude Code's 80.8% SWE-bench vs Codex's ~77%

**Accuracy:** Very high. Multi-step determinism praised in real-world use.

**Latency:** Very fast — 240+ tok/s; leads terminal-bench.

**Token efficiency:** High. Repository-aware context management.

**IDE support:** VS Code, JetBrains (native in AI chat), terminal CLI

**MCP:** Partial

**Privacy:** Cloud (OpenAI). No BYOK, no local models. Code sent to OpenAI.

**Limitations:**
- OpenAI models only — hard vendor lock-in
- No local model or BYOK support
- Requires paid ChatGPT subscription for serious use
- Less community support than Claude Code or OpenCode

---