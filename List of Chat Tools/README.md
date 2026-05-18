# AI Coding Agents — Complete Reference

> **May 2026** · For developers, QA, and DevOps — all skill levels, all tech stacks.

This document covers **18 AI coding agents** across every category: IDE-native editors, IDE extensions, terminal/CLI agents, autonomous cloud agents, and enterprise tools. Use it to pick the right tool for your workflow, understand trade-offs before committing, and build a stack that doesn't overlap.

---

## Contents

1. [How to read this document](#1-how-to-read-this-document)
2. [Agent categories explained](#2-agent-categories-explained)
3. [Master comparison table](#3-master-comparison-table)
4. [Per-agent profiles](#4-per-agent-profiles)
5. [Role-based picking guide](#5-role-based-picking-guide)
6. [Combinations that work](#6-combinations-that-work)
7. [Pricing summary](#7-pricing-summary)

---

## 1. How to read this document

Start with the **master comparison table** (Section 3) for a fast overview. Then read the **per-agent profile** for any tool you are seriously considering. Use the **role-based guide** (Section 5) if you want a direct recommendation by job function. Use **Section 6** to see what real teams actually run together.

---

## 2. Agent categories explained

| Category | What it means | Examples |
|---|---|---|
| **IDE Agent** | AI-native editor — the whole IDE is rebuilt around AI | Cursor, Windsurf, Kiro |
| **IDE Extension** | Plugin that adds AI to your existing editor | Copilot, Cline, Continue.dev, Tabnine, Cody, Amazon Q, JetBrains AI |
| **Terminal / CLI** | Runs in the terminal alongside any editor | Claude Code, Codex CLI, OpenCode, Aider, Gemini CLI |
| **Autonomous / Cloud** | You assign a task, it runs asynchronously in a sandbox | Devin, OpenHands |

> Most tools in 2026 span multiple categories. The label reflects their primary design intent.

---

## 3. Master Comparison Table

> Columns: **SWE** = SWE-bench Verified score (higher = better autonomous issue resolution). **BYOK** = Bring Your Own API Key. **OSS** = Open Source. Scores marked `~` are approximate or self-reported.

| Agent | Category | Model(s) | SWE-bench | Latency | Token Efficiency | Popularity | IDE Compatibility | MCP | Free Tier | Privacy | Repo / Docs | Summary/installation/examples |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **GitHub Copilot** | Extension + Chat | GPT-5.x, Claude, Gemini (multi-model) | ~80%* | Fast | Medium | 20M+ users, ~42% market share | VS Code, JetBrains, Neovim, Visual Studio, Xcode | Yes | Yes (2,000 completions/mo) | Cloud (opt-out of training available) | [GitHub Copilot Docs](https://docs.github.com/en/copilot) | [Copilot](Copilot) |
| **Claude Code** | Terminal / CLI | Claude Sonnet 4.6 / Opus 4.6 | 80.8% | Fast | Very High (5.5× fewer tokens than Cursor) | Growing fast; bundled with Claude Pro | Any editor via terminal; VS Code, JetBrains extensions | Yes (native) | No (Pro $20/mo) | Cloud (Anthropic) | [Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code/overview) | [Claude](Claude) |
| **Cursor** | IDE Agent | Claude, GPT-5.x, Gemini (multi-model) | ~80%* | Very Fast (<200ms inline) | Medium | 360K+ paying users | VS Code-based (standalone) | Yes | Yes (limited) | Cloud; code indexed on servers | [Cursor Docs](https://docs.cursor.com) | [Cursor](Cursor) |
| **Windsurf** | IDE Agent | SWE-1.5, Claude, GPT (multi-model) | ~78%* | Fast (950 tok/s via Cerebras) | High | Millions of users; acquired by Cognition | VS Code-based (standalone) | Yes | Yes | Cloud | [Windsurf Docs](https://docs.codeium.com/windsurf/getting-started) | [Windsurf](Windsurf) |
| **Kiro** | IDE Agent | Claude (via Amazon Bedrock) | N/A | Medium | High (spec-driven; precise scope) | New (AWS preview 2025) | VS Code-based (standalone); JetBrains via ACP | Yes | Yes (50 interactions/mo) | AWS Bedrock; GovCloud available | [Kiro Docs](https://kiro.dev/docs) | [Kiro](Kiro) |
| **OpenAI Codex** | Extension + Terminal | GPT-5.x | ~77%* | Fast (240+ tok/s) | High | Growing; used inside ChatGPT ecosystem | VS Code, JetBrains, terminal CLI | Partial | Yes (CLI is OSS) | Cloud (OpenAI); no BYOK | [OpenAI Codex Docs](https://platform.openai.com/docs/codex) | [Codex](Codex) |
| **Cline** | IDE Extension | Any (BYOK: Claude, GPT, local) | ~57% (with Claude) | Medium | High (Plan/Act stages) | 57K+ GitHub stars | VS Code | Yes (full) | Yes (OSS, BYOK) | Cloud or BYOK depending on provider | [Cline GitHub Repository](https://github.com/cline/cline) | [Cline](Cline) |
| **Continue.dev** | IDE Extension | Any (BYOK: Claude, GPT, Ollama, etc.) | Depends on model | Medium | High (model-dependent) | 31K+ GitHub stars | VS Code, JetBrains | Yes | Yes (OSS, BYOK) | Local / BYOK (full control) | [Continue.dev Repository](https://github.com/continuedev/continue) | [Continue.dev](Continue.dev) |
| **OpenCode** | Terminal / CLI | Any (75+ providers; BYOK) | ~77%* | Medium (22% slower than Codex CLI) | High | Growing open-source adoption | Any editor via terminal; desktop app; IDE extensions | Yes | Yes (OSS, BYOK) | Local-first; code never leaves machine unless using cloud LLM | [OpenCode Repository](https://github.com/sst/opencode) | [OpenCode](OpenCode) |
| **Aider** | Terminal / CLI | Any (BYOK: Claude, GPT, Ollama, etc.) | ~74%* (with Claude) | Medium | High (repo-map context) | 41K+ GitHub stars | Any editor (filesystem-level) | No | Yes (OSS, BYOK) | Local / BYOK | [Aider Repository](https://github.com/Aider-AI/aider) | [Aider](Aider) |
| **Gemini CLI** | Terminal / CLI | Gemini 3.x | ~80%* | Fast | High | 104K GitHub stars | Any editor via terminal | Yes | Yes (1,000 requests/day free) | Cloud (Google); Apache-2.0 | [Gemini CLI Repository](https://github.com/google-gemini/gemini-cli) | [Gemini Cli](Gemini%20Cli) |
| **Devin** | Autonomous / Cloud | Proprietary (Cognition) | 13.86% (2024 arch) | Slow (async; minutes to hours) | N/A (autonomous) | Enterprise; ~$4B valuation (2025) | Browser + Slack/Teams interface | No | No ($20/mo Core) | Cloud (Cognition); zero-retention on Pro+ | [Devin Official Site](https://devin.ai) | [Devin](Devin) |
| **OpenHands** | Autonomous / Cloud | Any (Claude, GPT, Gemini, local) | ~77%+ | Slow (async) | Medium | 73K+ GitHub stars | Web UI, CLI, desktop app | Yes | Yes (free via GitHub login; Minimax model) | Local Docker or Cloud; MIT license | [OpenHands Repository](https://github.com/All-Hands-AI/OpenHands) | [OpenHands](OpenHands) |
| **Tabnine** | IDE Extension | Proprietary + optional BYOK | N/A (completion-focused) | Very Fast (<50ms) | Medium | Enterprise-focused; millions of seats | VS Code, JetBrains, Vim, Neovim, Eclipse, all major IDEs | No | No (30-day trial) | On-prem / air-gapped available | [Tabnine Docs](https://docs.tabnine.com) | [Tabnine](Tabnine) |
| **Amazon Q Developer** | IDE Extension | Amazon Bedrock (Titan, Claude) | Top SWE-bench (agentic) | Fast | Medium | Enterprise AWS users | VS Code, JetBrains, Visual Studio, AWS Console | Partial | Yes (free tier with limits) | AWS; no data retention by default | [Amazon Q Developer Docs](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html) | [Amazon Q](Amazon%20Q) |
| **Sourcegraph Cody** | IDE Extension | Claude, GPT, Gemini, local (switchable) | N/A (search-focused) | Medium | Very High (Sourcegraph graph context) | Enterprise; 16K+ enterprise deployments | VS Code, JetBrains, Neovim, web | Partial | Yes (free on sourcegraph.com) | Cloud or self-hosted | [Sourcegraph Cody Docs](https://sourcegraph.com/docs/cody) | [Sourcegraph Cody](Sourcegraph%20Cody) |
| **JetBrains AI Assistant** | IDE Extension | Multiple (JetBrains-hosted + BYOK) | N/A | Fast | Medium | 12M+ JetBrains users | JetBrains IDEs only (IntelliJ, PyCharm, WebStorm, GoLand, etc.) | Partial | No ($10/mo add-on or included in some plans) | Cloud (JetBrains); EU servers option | [JetBrains AI Assistant Docs](https://www.jetbrains.com/ai/) | [JetBrains AI Assistant](JetBrains%20AI%20Assistant) |

> \* Scores marked `~` are benchmark-derived estimates or reported via the tool's underlying model, not tool-specific verified scores.  
> Devin's 13.86% reflects its 2024 architecture; Devin 2.0 improvements have not been independently benchmarked as of May 2026.

---

# 4. Role-based Picking Guide

### Backend Developer
| Priority | Recommended |
|---|---|
| Daily coding (VS Code) | Cursor or Copilot |
| Daily coding (JetBrains) | JetBrains AI Assistant + Cline |
| Deep reasoning / architecture | Claude Code |
| Terminal-first workflow | Aider or OpenCode |
| AWS-heavy backend | Amazon Q + Claude Code |
| Privacy-required | Tabnine (enterprise) or Continue.dev + Ollama |

### Frontend / UI Developer
| Priority | Recommended |
|---|---|
| Component-level coding | Cursor (best multi-file IDE) |
| Budget-conscious | Windsurf ($15/mo) |
| Mixed team (VS Code + JetBrains) | Continue.dev (supports both) |
| Architecture understanding | Sourcegraph Cody (if large codebase) |

### QA Engineer
| Priority | Recommended |
|---|---|
| Test generation | Claude Code or Copilot |
| Exploring unfamiliar code | Codeflow (browser, no setup) or Graphify |
| Autonomous test writing | OpenHands (delegate and review) |
| Understanding large legacy codebase | Sourcegraph Cody |

### DevOps / Platform Engineer
| Priority | Recommended |
|---|---|
| AWS infra / IaC | Amazon Q Developer |
| Terminal-first | Gemini CLI (free) or Aider |
| CI/CD task automation | OpenHands (headless/CLI mode) |
| Privacy / air-gapped | Tabnine Enterprise |
| General infra scripting | Claude Code or OpenCode |

---

## 5. Combinations That Work

These are the stacks real teams run in 2026:

**The most common professional setup ($30–40/mo)**
```
GitHub Copilot Pro ($10/mo)     — inline completions, quick chat, everywhere
Claude Code Pro ($20/mo)        — deep reasoning, complex tasks, architecture
```
Covers ~95% of daily coding scenarios.

**The budget-zero setup**
```
Gemini CLI (free, 1,000 req/day)  — terminal agent for tasks
Continue.dev + Ollama (free)      — IDE assistant with local models
Codeflow (free, browser)          — architecture visualisation
```
Genuinely usable at $0.

**The JetBrains-native stack**
```
JetBrains AI Assistant ($10/mo)   — native IDE integration
Cline or Continue.dev (free)      — agentic tasks, BYOK
Claude Code ($20/mo)              — complex reasoning, terminal
```

**The privacy-first enterprise stack**
```
Tabnine Enterprise (on-prem)      — completions, no cloud
Continue.dev + local Ollama       — IDE chat, fully local
Aider + local model               — terminal agent, fully local
```

**The autonomous delegation stack**
```
Cursor Pro ($20/mo)               — daily IDE work
Claude Code ($20/mo)              — complex reasoning
OpenHands (free)                  — autonomous task delegation
```

**The AWS team stack**
```
Amazon Q Developer ($19/mo)       — AWS-native coding and infra
Kiro ($19/mo)                     — spec-driven feature development
Claude Code ($20/mo)              — deep reasoning and architecture
```

---

## 6. Pricing Summary

| Agent | Free Tier | Entry Paid | Heavy Use |
|---|---|---|---|
| GitHub Copilot | 2,000 completions/mo | $10/mo (Pro) | $19/user/mo (Business) |
| Claude Code | No | $20/mo (Pro) | $100–200/mo (real heavy use) |
| Cursor | Yes (limited) | $20/mo (Pro, credit-based) | $60/mo (Pro+) · $200/mo (Ultra) |
| Windsurf | Yes (limited) | $15/mo (Pro) | $35/user/mo (Teams) |
| Kiro | 50 interactions/mo | $19/mo (Pro) | $39/mo (Pro+) |
| OpenAI Codex | CLI is free (OSS) | Needs ChatGPT Plus ($20/mo) | $200/mo (ChatGPT Pro) |
| Cline | Yes (OSS, BYOK) | API costs only ($10–30/mo typical) | Scales with API usage |
| Continue.dev | Yes (OSS, BYOK) | API costs only | Scales with API usage |
| OpenCode | Yes (OSS, BYOK) | API costs only ($10–30/mo typical) | Scales with API usage |
| Aider | Yes (OSS, BYOK) | API costs only | Scales with API usage |
| Gemini CLI | 1,000 req/day free | API costs (very low) | Gemini API pricing |
| Devin | No (trial only) | $20/mo (Core) | $500/user/mo (Team) |
| OpenHands | Yes (GitHub login, Minimax) | API costs only | Enterprise: contact sales |
| Tabnine | 30-day trial only | $9/mo (Dev) | Enterprise: custom |
| Amazon Q Developer | Yes (limited) | $19/user/mo (Pro) | Enterprise: custom |
| Sourcegraph Cody | Yes (sourcegraph.com) | $9/mo (Pro) | $19/user/mo (Enterprise) |
| JetBrains AI | Yes (students/OSS) | $10/mo add-on | Included in some All Products Pack plans |

---

## Notes

- Prices are as of May 2026. AI tool pricing changes frequently. Verify on each vendor's site before purchasing.
- SWE-bench scores represent the tool or its primary model's performance on autonomous GitHub issue resolution. Higher = better.
- "BYOK" tools (Cline, Continue.dev, OpenCode, Aider) appear cheap but API costs add up — budget $10–50/mo depending on usage.
- Tools marked OSS are open source. Licenses vary: MIT (OpenCode, OpenHands), Apache 2.0 (Gemini CLI, Codex CLI, Aider, Continue.dev, Cline).
- MCP (Model Context Protocol) support enables tools to connect to external data sources, APIs, and knowledge graphs. Prefer MCP-compatible tools for graph-augmented workflows.

---

*Generated May 2026. Sources: Stack Overflow Developer Survey 2025, SWE-bench Leaderboard, vendor documentation, community benchmarks.*