### Kiro

**Type:** IDE Agent (VS Code fork, by AWS)  
**Model:** Claude (via Amazon Bedrock)  
**Pricing:** Free (50 interactions/mo) · Pro $19/mo (225 vibe + 125 spec requests) · Pro+ $39/mo  
**Repo/Docs:** https://kiro.dev/docs/

AWS's spec-driven AI IDE. Before writing a single line of code, Kiro generates three documents: `requirements.md`, `design.md`, and `tasks.md`. This structured approach catches design mistakes early and produces more consistent, reviewable output than vibe-coding tools. Does not require an AWS account — login with GitHub or Google. Deep native integration with Lambda, CDK, CloudFormation, and CodeCatalyst for AWS-native teams. Agent hooks (event-driven automations in natural language) trigger AI actions when files are saved, committed, or changed. Available in AWS GovCloud with FedRAMP High authorization.

**When to use:**
- You work on AWS-native infrastructure or applications
- Your team values structured, spec-first development over rapid vibe-coding
- You need compliance, auditability, or governance (GovCloud, FedRAMP)
- You want to enforce consistent standards across junior and senior developers

**When NOT to use:**
- You want the fastest tool — spec generation adds upfront time
- You are not on AWS and have no plan to be
- You need the most powerful reasoning model — Claude Code accesses Claude more directly

**Accuracy:** High for structured, scoped tasks. Less tested on raw benchmark tasks.

**Latency:** Medium. Spec generation is intentionally deliberate, not instant.

**Token efficiency:** High. Spec-driven scope limits reduces irrelevant context.

**IDE support:** VS Code-based (standalone); JetBrains and other ACP-compatible editors via Kiro CLI

**MCP:** Yes — native MCP support for connecting external tools.

**Privacy:** AWS Bedrock (SOC 2, FedRAMP). No AWS account required for basic use.

**Limitations:**
- Spec workflow can feel heavy for quick, simple tasks
- Still relatively new — smaller community than Cursor or Copilot
- Locked to Claude via Bedrock — no multi-model flexibility
- Autopilot mode (default) may surprise new users with autonomous actions

---
