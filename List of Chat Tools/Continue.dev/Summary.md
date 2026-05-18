### Continue.dev

**Type:** IDE Extension  
**Model:** Any (BYOK: Claude, GPT, Gemini, Ollama, any OpenAI-compatible endpoint)  
**Pricing:** Free (Apache 2.0 open source). Pay only for LLM API calls.  
**Repo/Docs:** https://github.com/continuedev/continue

The most configurable open-source IDE extension. Works in both VS Code and JetBrains — one of very few tools with first-class support for both. Connects to any model, any API endpoint, including fully local Ollama models for zero cloud exposure. Custom slash commands, context providers (docs, web, codebase), and deeply configurable `.continue/config.json`. Best for teams that want to assemble their own AI stack with full transparency.

**When to use:**
- JetBrains AND VS Code users on the same team need the same tool
- You want full model and provider flexibility with zero lock-in
- Local models (Ollama/LM Studio) for privacy-sensitive codebases
- You want a free, customisable base that you control entirely

**When NOT to use:**
- You want plug-and-play simplicity — Continue requires configuration
- You want an autonomous agent — Continue is an assistant, not an agent
- You need the highest possible accuracy — quality tracks your chosen model

**Accuracy:** Model-dependent. With Claude or GPT-5 backend: excellent. With weak local models: limited.

**Latency:** Model-dependent. Local models can be slower; cloud models match other tools.

**Token efficiency:** High — context providers let you inject only what's relevant.

**IDE support:** VS Code, JetBrains (first-class for both)

**MCP:** Yes

**Privacy:** Local / BYOK. With Ollama: fully air-gapped. Code only leaves machine when using a cloud model.

**Limitations:**
- No built-in autocomplete (separate tab-completion feature in beta)
- Not autonomous — it assists, not delegates
- Manual configuration can be complex for non-technical users
- Documentation still catching up with feature set

---