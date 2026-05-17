### Aider

**Type:** Terminal / CLI Agent  
**Model:** Any (BYOK: Claude, GPT, Gemini, Ollama, any OpenAI-compatible API)  
**Pricing:** Free (Apache 2.0). Pay only for API calls.  
**Repo/Docs:** https://github.com/paul-gauthier/aider

The only tool in this list that is Git-native by design: every change is automatically committed with a sensible message. You run `aider` from your project directory, describe what you want, and it edits files and commits. No IDE required. No browser. No extension. It maps your entire repo structure as a tree (repo-map) that it uses to understand scope without reading every file. 41K+ GitHub stars. 93+ releases. The most mature terminal coding agent available. Widely used by backend engineers, DevOps, and terminal-first developers.

**When to use:**
- You live in the terminal and do not want to touch an IDE
- Git-first workflow — every change auto-committed with a message
- You want the most mature, battle-tested terminal agent
- You want to work with any model, any editor, zero lock-in

**When NOT to use:**
- You need inline IDE suggestions — Aider does not provide completions
- You need visual feedback on changes — terminal only
- Very large monorepos — repo-map can hit memory limits
- You need autonomous background execution — Aider is interactive

**Accuracy:** Very high with Claude Sonnet/Opus (~74% SWE-bench with Claude). Model-dependent.

**Latency:** Medium. Terminal interaction; no streaming inline completion.

**Token efficiency:** High. Repo-map provides structural context without reading all files. Similar philosophy to knowledge graphs.

**IDE support:** Any editor (filesystem-level; editor-agnostic)

**MCP:** No

**Privacy:** Local / BYOK. With Ollama: fully private. Git commits on local machine.

**Limitations:**
- Terminal only — no visual interface
- No inline autocomplete
- Repo-map can hit memory limits on very large repos (200k+ files)
- No web UI or desktop app

---