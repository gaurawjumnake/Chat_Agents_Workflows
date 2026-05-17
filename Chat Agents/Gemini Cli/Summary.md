### Gemini CLI

**Type:** Terminal / CLI Agent  
**Model:** Gemini 3.x (Google)  
**Pricing:** Free (1,000 requests/day via personal Google account). Paid: Gemini API pricing applies.  
**Repo/Docs:** https://github.com/google-gemini/gemini-cli

The most generous free tier of any AI coding agent. 1,000 requests/day is effectively unlimited for most developers. 104K GitHub stars. Apache-2.0 open source. Runs in the terminal alongside any editor. Gemini 3.1 Pro gives it the largest context window in this list (1M tokens) and strong reasoning (GPQA Diamond 94.3%). MCP support means it can connect to the same tools as Claude Code and Copilot. If you are budget-constrained, this is where to start.

**When to use:**
- Budget is zero — 1,000 free requests/day is genuinely usable
- You need the largest context window (1M tokens — ideal for dumping an entire codebase)
- You want an open-source CLI agent without vendor lock-in
- Google Cloud / GCP teams (native integration)

**When NOT to use:**
- You need the highest coding accuracy — Claude Code leads SWE-bench
- You want inline IDE autocomplete — terminal only
- Privacy is critical — code sent to Google

**Accuracy:** High. Gemini 3.1 Pro is a frontier model. 94.3% GPQA Diamond (best reasoning benchmark in this list).

**Latency:** Fast. Gemini 3 Flash option for speed-sensitive workflows.

**Token efficiency:** Very High. 1M context window handles entire large repos without chunking.

**IDE support:** Any editor via terminal

**MCP:** Yes

**Privacy:** Cloud (Google). Apache-2.0 license for the tool itself.

**Limitations:**
- Locked to Gemini models — no BYOK for other providers
- Code sent to Google Cloud
- Terminal only — no IDE integration
- Gemini's coding accuracy still trails Claude Code on complex tasks

---