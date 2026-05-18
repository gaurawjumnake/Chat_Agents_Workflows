### Cline

**Type:** IDE Extension (VS Code)  
**Model:** Any (BYOK: Claude, GPT, Gemini, Ollama, local models)  
**Pricing:** Free (open source). You pay only for API calls to your chosen provider. Typical: $10–30/mo in API costs.  
**Repo/Docs:** https://github.com/cline/cline

The leading open-source VS Code agent. 57K+ GitHub stars. Plan/Act workflow: agent proposes a plan you review before it executes. Always asks for confirmation before risky actions — strong governance model. Full MCP integration for connecting to external tools and data sources. BYOK means full model flexibility and cost control. Step-by-step approval makes it the safest agentic tool for teams with compliance needs.

**When to use:**
- You want a capable VS Code agent with full model flexibility
- Safety and approval-at-every-step matter (regulated environments, junior devs)
- You want BYOK and zero per-seat subscription
- You need MCP tool connections inside VS Code

**When NOT to use:**
- Constant approval prompts slow you down — use a more autonomous agent for delegated tasks
- You need JetBrains support — VS Code only
- You want inline autocomplete — Cline is task-level, not keystroke-level

**Accuracy:** Depends on model. With Claude Sonnet 4.6: ~57% SWE-bench.

**Latency:** Medium. Plan/Act stages add confirmation steps.

**Token efficiency:** High. Plan stage scopes context before execution.

**IDE support:** VS Code only

**MCP:** Yes — full support

**Privacy:** Code goes to whichever LLM provider you configure. Local models via Ollama = fully private.

**Limitations:**
- Approval-required-on-every-action can be slow for autonomous workflows
- VS Code only
- Quality entirely dependent on the model you connect
- Setup requires more configuration than plug-and-play tools

---