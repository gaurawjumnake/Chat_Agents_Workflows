### Sourcegraph Cody

**Type:** IDE Extension + Agentic Layer (Amp)  
**Model:** Claude, GPT, Gemini, local via Ollama (switchable)  
**Pricing:** Free (Sourcegraph.com) · Pro $9/mo · Enterprise $19/user/mo (+ Sourcegraph license)  
**Repo/Docs:** https://github.com/sourcegraph/cody

The best tool for understanding very large, complex codebases. Built on Sourcegraph's code search and graph engine — indexes millions of lines across hundreds of repositories and uses that graph for AI context. Designed for Fortune 500 companies with massive legacy codebases that other tools cannot handle. The new Amp layer (free) adds agentic planning and execution on top of Cody's search-powered context. Model-switchable: use Claude for depth, GPT for speed, or local models for privacy.

**When to use:**
- Your codebase has millions of lines across many repos — other tools lose context at this scale
- Multi-repo, polyglot enterprise environments
- You need AI that understands decades of legacy code
- Onboarding new engineers to a complex, undocumented codebase

**When NOT to use:**
- Small-to-medium codebases — the Sourcegraph overhead is not worth it
- Greenfield projects — Cody's strength is understanding existing code, not writing new code from scratch
- Budget-conscious teams — full power requires Sourcegraph + Cody licenses

**Accuracy:** Very high for codebase Q&A and navigation. Graph-powered context gives it the most accurate structural understanding of any tool here.

**Latency:** Medium. Graph indexing happens in the background; queries are fast.

**Token efficiency:** Very High. Sourcegraph graph provides precisely scoped context — similar benefit to knowledge graph tools, but at enterprise scale.

**IDE support:** VS Code, JetBrains, Neovim, web editor

**MCP:** Partial

**Privacy:** Cloud or self-hosted. Enterprise single-tenant deployment available.

**Limitations:**
- Full capability requires Sourcegraph license (enterprise pricing)
- Less useful for greenfield development
- Heavier setup than extensions like Copilot or Cline
- Smaller community than Cursor or Copilot

---