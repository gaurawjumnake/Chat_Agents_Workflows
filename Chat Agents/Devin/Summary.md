### Devin

**Type:** Autonomous / Cloud Agent  
**Model:** Proprietary (Cognition AI)  
**Pricing:** Core $20/mo · Team $500/user/mo + $2/ACU · Enterprise: contact sales  
**Repo/Docs:** https://devin.ai

The original "AI software engineer" — the first agent to autonomously resolve GitHub issues at scale. Works asynchronously: assign a task via Slack, Teams, or the Devin interface, and it executes in a cloud sandbox (terminal + browser + editor) and submits a PR when done. Devin 2.0 (April 2025) dropped the individual price from $500/mo to $20/mo. Builds a confidence score before starting, letting you decide if the task is worth running. Self-healing: when code fails tests, Devin reads the error, iterates, and fixes without human intervention.

**When to use:**
- Fully scoped, unambiguous tasks where you want to delegate and review output later
- Legacy code migration at scale (COBOL, Objective-C → modern languages)
- Teams that can absorb $500/seat for genuine autonomous delegation
- Tasks that would take a junior developer 30–60 minutes with clear specs

**When NOT to use:**
- Complex, ambiguous, or architectural tasks — Devin's real-world success rate on these is ~14–15%
- Budget-conscious teams — $500/seat is steep; OpenHands is a capable free alternative
- Tasks requiring real-time collaboration or tight human oversight
- Production database access — always use staging/local environments with Devin

**Accuracy:** 13.86% SWE-bench (2024 architecture; Devin 2.0 improved but no updated benchmark published). Real-world success ~14–15% on complex tasks; higher on well-scoped simple tasks.

**Latency:** Slow — async cloud execution; tasks take minutes to hours.

**Token efficiency:** N/A — fully autonomous; no per-prompt token management.

**IDE support:** Browser interface, Slack/Teams integration. No IDE plugin.

**MCP:** No

**Privacy:** Cloud (Cognition). Zero-retention on Pro and Enterprise plans. Sandbox environment.

**Limitations:**
- Real-world autonomous success rate is low (~14–15%) on complex tasks
- Very expensive at Team tier ($500/user/mo)
- No control over architectural decisions mid-task
- Cannot access production systems safely — sandbox only
- No updated SWE-bench figures for Devin 2.0

---