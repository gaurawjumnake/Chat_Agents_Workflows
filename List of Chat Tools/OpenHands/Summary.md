### OpenHands

**Type:** Autonomous / Cloud Agent + Local Option  
**Model:** Any (Claude, GPT, Gemini, Ollama, LiteLLM; full BYOK)  
**Pricing:** Free (MIT open source). Cloud tier free with GitHub login (Minimax model). Enterprise: contact sales.  
**Repo/Docs:** https://github.com/OpenHands/OpenHands

Open-source Devin alternative with 73K+ GitHub stars. The most complete free autonomous agent available. Three interface options: Web UI (like Devin), CLI (like Claude Code), and desktop app. Runs in Docker locally for full privacy, or on OpenHands Cloud for zero-setup. Planning Mode (v1.6.0) lets the agent write a reviewable plan before executing. Kubernetes support for multi-user team deployments. SDK for building custom agents on top of OpenHands infrastructure. Resolves 50%+ of real GitHub issues (reported with Claude Sonnet).

**When to use:**
- You want Devin-like autonomous delegation without the $500/mo price
- You need BYOK flexibility — run any model, including local
- You want to self-host for privacy or compliance
- Building custom agent workflows using the SDK

**When NOT to use:**
- You need tight IDE integration or inline completions — OpenHands is task-level
- You cannot run Docker (restricted corporate laptops)
- You need enterprise SLA and support — use Devin or OpenHands Enterprise

**Accuracy:** ~77%+ SWE-bench with Claude Sonnet. Score drops with smaller/cheaper models.

**Latency:** Slow — async autonomous execution (minutes per task).

**Token efficiency:** Medium — autonomous execution has less predictable token usage.

**IDE support:** Web UI, CLI, desktop app. No inline IDE extension.

**MCP:** Yes

**Privacy:** Local Docker (fully private) or OpenHands Cloud. MIT license.

**Limitations:**
- Requires Docker — blocked on many corporate laptops
- Errors can compound before you see output — review commits carefully
- Score drops significantly with smaller models
- Documentation still maturing

---