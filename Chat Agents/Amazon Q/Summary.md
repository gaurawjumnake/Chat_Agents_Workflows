### Amazon Q Developer

**Type:** IDE Extension + Cloud Agent  
**Model:** Amazon Bedrock (Titan, Claude)  
**Pricing:** Free tier (with monthly limits) · Pro $19/user/mo  
**Repo/Docs:** https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html

Amazon's AI coding assistant (evolved from CodeWhisperer). Deep AWS-native integration: generates contextually accurate IAM policies, CloudFormation templates, CDK code, SDK calls, and CLI commands. Built-in security vulnerability scanning. License-aware completions (flags code that may infringe on open-source licenses). Agentic coding (multi-file feature implementation, test generation, documentation). Works in VS Code, JetBrains, Visual Studio, and the AWS Console as a chat widget. Topped SWE-bench leaderboard for its agentic tier (Amazon Q's reported score).

**When to use:**
- You build on AWS — this is the only tool with deep AWS service context
- You need licence-aware code generation (legal/compliance requirement)
- You want a free tier with genuinely useful capabilities
- Security scanning as part of the coding workflow

**When NOT to use:**
- Your stack is not AWS — Q's advantages are AWS-specific; general coding is not its strength
- You need local or BYOK model access
- You need the most advanced autonomous agent — Devin or OpenHands are stronger

**Accuracy:** High for AWS-specific tasks. General coding solid but not frontier-level.

**Latency:** Fast — backed by AWS infrastructure.

**Token efficiency:** Medium.

**IDE support:** VS Code, JetBrains, Visual Studio, AWS Console, CLI

**MCP:** Partial

**Privacy:** AWS infrastructure. No data retention by default. Private endpoint option for regulated environments.

**Limitations:**
- AWS-centric — limited value for non-AWS teams
- No BYOK — locked to Bedrock models
- General coding quality behind Cursor or Claude Code
- Replaced CodeWhisperer — some legacy plugin confusion during migration

---