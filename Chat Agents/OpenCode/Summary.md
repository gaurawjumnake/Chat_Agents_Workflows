### OpenCode

**Type:** Terminal / CLI Agent  
**Model:** Any (75+ providers via Models.dev: Claude, GPT, Gemini, Ollama, and more)  
**Pricing:** Free (MIT open source). Pay only for API calls. ~$10–30/mo typical cloud usage.  
**Repo/Docs:** https://github.com/sst/opencode

The most-starred open-source AI coding tool at 153K+ GitHub stars. Built by the SST team as the open-source alternative to Claude Code — same terminal-native experience, but not locked to any vendor. Two built-in agents: `build` (full file read/write/execute access) and `plan` (read-only analysis, no changes). Switch with Tab. Client/server architecture means you can run the server remotely and drive it from terminal, desktop app, or IDE extension. Privacy-first: code never leaves your machine unless you explicitly use a cloud LLM.

**When to use:**
- You want Claude Code-style terminal workflow without Anthropic lock-in
- You need vendor flexibility — swap models without workflow disruption
- Privacy-sensitive codebases (fully local with Ollama)
- Large teams where per-seat SaaS costs are significant

**When NOT to use:**
- You want inline autocomplete — OpenCode is task-level only
- You prefer a GUI — terminal TUI (though a desktop app exists)
- You need the highest accuracy on complex tasks — Claude Code's reasoning depth still leads
- Windows users (WSL works but native Windows is less polished)

**Accuracy:** High and model-dependent. Slightly slower than Codex CLI (~22%) but stronger privacy guarantees.

**Latency:** Medium — 22% slower than Codex CLI on equivalent tasks. Acceptable for most workflows.

**Token efficiency:** High. Models.dev integration and local-first design minimise unnecessary API calls.

**IDE support:** Any editor via terminal; desktop app; IDE extensions for VS Code and JetBrains

**MCP:** Yes

**Privacy:** Local-first. Code stays on machine when using local models. Cloud LLM usage is explicit and user-initiated.

**Limitations:**
- 22% slower than Codex CLI on equivalent benchmarks
- Setup requires more steps than plug-and-play tools
- No built-in autocomplete
- Community smaller than commercial tools like Cursor

---