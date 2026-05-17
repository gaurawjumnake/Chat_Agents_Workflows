### Cursor

**Type:** IDE Agent (VS Code fork)  
**Model:** Claude (default), GPT-5.x, Gemini, proprietary Composer model  
**Pricing:** Free (limited) · Pro $20/mo (credit-based) · Pro+ $60/mo · Ultra $200/mo  
**Repo/Docs:** https://docs.cursor.com

The benchmark for AI-native IDE experience. 360K+ paying users. Composer (Cmd+I) opens a multi-file editing interface. Up to 8 parallel background agents. Sub-200ms inline completions. Because it is VS Code-based, all VS Code extensions work. The default choice for developers who want IDE-first agentic coding.

**When to use:**
- You want the best day-to-day AI-native IDE experience
- Multi-file refactors that need visual, in-editor feedback
- You already use VS Code and want a near-seamless transition
- Team use — best ecosystem, most tutorials, most community support

**When NOT to use:**
- You use JetBrains exclusively — Cursor is VS Code only
- You need deep reasoning over huge context — Claude Code is stronger here
- Budget predictability matters — credit-based billing can surprise at scale

**Accuracy:** Very high. Tops most IDE-level benchmarks. Multi-model access means you route to the best model per task.

**Latency:** Very fast. Sub-200ms inline. Background agents run asynchronously.

**Token efficiency:** Medium. Indexing is powerful but not as token-lean as Claude Code's graph-based approach.

**IDE support:** VS Code only (Cursor is the editor itself)

**MCP:** Yes

**Privacy:** Cloud. Code indexed on Cursor servers. Enterprise plan adds data isolation.

**Limitations:**
- VS Code only — no JetBrains
- Credit billing makes heavy automated workflows expensive
- Can feel overwhelming for simple tasks
- Tight coupling to Cursor's model menu — less flexible than pure BYOK tools

---