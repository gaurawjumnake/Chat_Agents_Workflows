### JetBrains AI Assistant

**Type:** IDE Extension  
**Model:** JetBrains-hosted models + BYOK (OpenAI, Anthropic)  
**Pricing:** Included in some JetBrains plans · Add-on: $10/mo · Free for students/OSS  
**Repo/Docs:** https://www.jetbrains.com/ai/

The native AI option for the 12M+ JetBrains user base (IntelliJ IDEA, PyCharm, WebStorm, GoLand, Rider, CLion, etc.). Integrates directly into JetBrains' code analysis engine — AI suggestions are aware of your project's type system, refactoring dialogs, and version control. No third-party extension needed. EU server option for data residency requirements. If your entire team lives in JetBrains IDEs and you do not want to add external tools, this is the path of least friction.

**When to use:**
- Your entire team uses JetBrains IDEs and you want zero added tooling
- You want AI tightly integrated with JetBrains' own refactoring and inspection engine
- EU data residency requirement — EU server option available
- Students or open-source contributors — free access

**When NOT to use:**
- You need frontier model accuracy — JetBrains-hosted models lag behind Claude/GPT-5
- You need autonomous or agentic features — JetBrains AI is an assistant, not an agent
- You use VS Code — JetBrains AI only works in JetBrains IDEs

**Accuracy:** Good for JetBrains-native tasks. Below frontier tools for complex reasoning.

**Latency:** Fast — native IDE integration with no context-switch.

**Token efficiency:** Medium.

**IDE support:** JetBrains IDEs only (IntelliJ, PyCharm, WebStorm, GoLand, Rider, CLion, DataGrip, etc.)

**MCP:** Partial

**Privacy:** Cloud (JetBrains servers). EU server option. BYOK option sends code to your chosen provider.

**Limitations:**
- JetBrains only — useless for VS Code users
- AI capabilities lag behind dedicated tools like Cursor or Claude Code
- Agentic features are limited compared to Cline or Copilot Agent Mode
- Model quality below frontier unless you configure BYOK

---