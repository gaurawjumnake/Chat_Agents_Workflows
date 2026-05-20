# AI-Assisted Software Delivery Workflows & Tool Guide

**Token-optimized, gate-enforced, spec-driven workflows for Developer and QA teams.**

This repository provides:
- **2 Complete Workflows** (Developer & QA) with phases, gates, agents, hooks, snippets, and persistent memory
- **20+ AI Chat Tools** with setup guides, examples, and usage patterns
- **Context & Graph Integration** for smarter code analysis and change impact assessment
- **UI/Frontend Extensions** for component-driven and accessibility-first development

---

## 🚀 Quick Navigation

| What I Need | Start Here |
|---|---|
| **Building a feature** | → [Developer Workflow](#developer-workflow-complete-implementation-cycle) |
| **Testing a release** | → [QA Workflow](#qa-workflow-complete-validation-cycle) |
| **Choosing a tool** | → [List of Chat Tools](#list-of-chat-tools-20-platforms) |
| **Token tips** | → [Token Optimization Guide](#token-optimization-and-context-strategy) |
| **Running prompts** | → [Prompt Sequence & Execution](#recommended-prompt-sequence--how-to-run) |

---

## 📋 Repository Contents At a Glance

### Root Level Files (Strategy & Guides)
- **1. Chat Agent Usage Guide.txt** — Token efficiency, graph-first thinking, prompting discipline
- **2. MODEL SELECTION GUIDE.txt** — Which model for which task (Copilot vs Claude vs Cursor, etc.)
- **4. prompt to build context.md** — How to create reusable context files for your codebase
- **5. prompt to merge context.md** — How to integrate context into all workflow phases
- **6.1-6.3 Graph Prompts** — Advanced graph-based code understanding (optional, requires tooling)
- **7. prompt to update QA workflow for ui-ux testing.md** — Add UI/UX validation to QA
- **8. prompt to update developer workflow for UI developer.md** — Add component-driven frontend work
- **CLIENT-PITCH-SUMMARY.md** — Executive summary of the system
- **README.md** — You are here

### Three Main Components
1. **[Developer Workflow](#developer-workflow-complete-implementation-cycle)** — Spec → Code → Test → Review → Release
2. **[QA Workflow](#qa-workflow-complete-validation-cycle)** — Requirement → Impact → Spec QA → Risk → Test → Security → Release
3. **[List of Chat Tools](#list-of-chat-tools-20-platforms)** — 20+ AI tools with setup and usage guides

---

## 1️⃣ Developer Workflow (Complete Implementation Cycle)

**Location**: `Developer Workflow/.github/.workflow/`

**Purpose**: Build features from requirement to production with mandatory spec approval and incremental testing.

### Core 10 Phases

| Phase | Purpose | Gate | Duration |
|-------|---------|------|----------|
| 1. Requirement Understanding | Parse and clarify requirements | none | 10-20 min |
| 2. Impact Analysis | What modules are affected? | none | 15-30 min |
| 3. Spec Creation | Write approved spec (mandatory) | **SPEC_APPROVED** | 30-60 min |
| 4. Design | Architecture and approach | none | 20-40 min |
| 5. Implementation | Write code per spec | none | Variable |
| 6. Testing | Unit/integration tests | none | 20-60 min |
| 7. Debugging | Fix failures and edge cases | **TESTS_PASS** | Variable |
| 8. Refactoring | Code quality and performance | none | 20-40 min |
| 9. Code Review | Peer review with gates | **CODE_APPROVED** | 15-30 min |
| 10. Documentation | Update docs and changelog | **RELEASE_READY** | 10-20 min |

### What's Inside

```
Developer Workflow/.github/.workflow/
├── workflow-orchestrator.md      ← Main entry point (all phases, gates, execution)
├── agents/                       (11 agents for deep analysis)
│   ├── requirement.agent.md
│   ├── impact.agent.md
│   ├── spec.agent.md
│   ├── design.agent.md
│   ├── implementation.agent.md
│   ├── test.agent.md
│   ├── debug.agent.md
│   ├── refactor.agent.md
│   ├── review.agent.md
│   ├── documentation.agent.md
│   └── (more...)
├── hooks/                        (Triggered execution points)
│   ├── pre-task/
│   ├── spec/
│   ├── implementation/
│   ├── pr/
│   └── (more by phase)
├── snippets/                     (Reusable prompt templates)
│   ├── create_spec.snippet.md
│   ├── implement_from_spec.snippet.md
│   ├── generate_tests.snippet.md
│   └── (more...)
├── ai-memory/                    (Persistent findings)
│   ├── bugs/                     (Known issues & fixes)
│   ├── patterns/                 (Code patterns to reuse)
│   ├── decisions/                (Architecture decisions)
│   └── specs/                    (Approved specs)
├── instructions/                 (Reference guidance)
│   ├── core/                     (Phase definitions, gates)
│   ├── workflow/                 (How to use workflow)
│   ├── token-optimization/       (Token budgets)
│   └── (more...)
└── specs/                        (Feature specifications)
    └── spec-template.md
```

### How to Use Developer Workflow

**Step 1: Start a Feature**
```
1. Open Developer Workflow/.github/.workflow/workflow-orchestrator.md
2. Go to Phase 1: Requirement Understanding
3. Follow execution steps (inputs, what to ask, gates)
```

**Step 2: Build a Spec**
```
1. After Phase 1-2, move to Phase 3: Spec Creation
2. Use snippets/create_spec.snippet.md to write spec
3. Get SPEC_APPROVED gate before implementing
```

**Step 3: Implement**
```
1. Phase 5: Implementation
2. Use snippets/implement_from_spec.snippet.md
3. Reference spec path, not requirements text
4. Use diffs for change analysis
```

**Step 4: Test & Iterate**
```
1. Phase 6: Testing (generate tests from spec)
2. Phase 7: Debugging (if failures)
3. Phase 8: Refactoring (code quality)
```

**Step 5: Review & Release**
```
1. Phase 9: Code Review (peer approval)
2. Phase 10: Documentation & Release
3. Archive decisions to ai-memory/
```

### Key Features

- ✅ **Mandatory spec approval** before implementation (prevents rework)
- ✅ **Incremental testing** during implementation (catch bugs early)
- ✅ **Reusable patterns** in memory (learn from similar features)
- ✅ **Token budgets** per phase (no bloated prompts)
- ✅ **Diff-focused** analysis (avoid full repo scans)
- ✅ **11 specialized agents** for each phase (deep reasoning)

### Example: Adding a Payment Retry Feature

```
Phase 1: "Retry transient payment failures up to 3 times"
Phase 2: "Affects payment-service, webhook-handler, retry-queue"
Phase 3: SPEC_APPROVED
Phase 4: "Use exponential backoff with jitter"
Phase 5: "Implement in payment-service/retry.go"
Phase 6: "All tests pass ✓"
Phase 7: "No failures"
Phase 8: "Code quality check"
Phase 9: CODE_APPROVED
Phase 10: "Changelog updated, ready for release"
```

---

## 2️⃣ QA Workflow (Complete Validation Cycle)

**Location**: `QA Workflow/.workflow/`

**Purpose**: Validate features from requirement through release with hard approval gates at each phase.

### Core 10 Phases

| Phase | Purpose | Gate | Duration |
|-------|---------|------|----------|
| 1. Requirement QA | Is it testable? | **REQUIREMENT_APPROVED** | 10-20 min |
| 2. QA Impact Analysis | What modules/tests are affected? | **IMPACT_MAPPED** | 15-30 min |
| 3. Spec QA Analysis | QA strategy and test matrix | **SPEC_QA_APPROVED** | 30-60 min |
| 4. Risk & Threat Analysis | What could go wrong? | **RISK_APPROVED** | 30-60 min |
| 5. Test Design & Execution | Run targeted tests | **TEST_RESULTS_READY** | Variable |
| 6. Test Execution & Debug | Root cause analysis | **FAILURES_TRIAGED** | Variable |
| 7. Regression Scope | Minimal regression subset | **REGRESSION_SCOPED** | 20-40 min |
| 8. Security Validation | Auth, RBAC, abuse paths | **SECURITY_PASS** | 30-60 min |
| 9. Healing Analysis | Fix flaky tests safely | **HEALING_APPROVED** | 20-40 min |
| 10. Release Validation | Go/No-Go decision | **RELEASE_APPROVED** | 15-30 min |

### What's Inside

```
QA Workflow/.workflow/
├── workflow-orchestrator.md      ← Main entry point (all phases, gates, execution)
├── agents/                       (12 agents for validation)
│   ├── requirement-qa.agent.md
│   ├── qa-impact.agent.md
│   ├── functional-test.agent.md
│   ├── api-test.agent.md
│   ├── risk.agent.md
│   ├── threat-model.agent.md
│   ├── bug-analysis.agent.md
│   ├── regression.agent.md
│   ├── security.agent.md
│   ├── healing.agent.md
│   ├── release-validation.agent.md
│   └── (more...)
├── hooks/                        (Triggered validation points)
│   ├── spec/
│   ├── test/
│   ├── regression/
│   ├── security/
│   ├── healing/
│   └── (more by phase)
├── snippets/qa/                  (Reusable QA templates)
│   ├── test_generation.snippet.md
│   ├── api_contract_test.snippet.md
│   ├── edge_case_generation.snippet.md
│   ├── security_validation.snippet.md
│   └── (more...)
├── qa-context/                   (Stable QA strategy)
│   ├── test-strategy.md          (Scope, entry/exit criteria)
│   ├── security-model.md         (Auth, RBAC, abuse paths)
│   ├── regression-map.md         (Test families by feature)
│   ├── api-test-matrix.md        (API coverage)
│   ├── flaky-tests.md            (Known flaky tests + fixes)
│   └── environments.md           (Test/staging/prod)
├── qa-memory/                    (Historical findings)
│   ├── bugs/                     (Bug patterns & fixes)
│   ├── security/                 (Security findings & remediation)
│   ├── healing/                  (Flaky test healing)
│   ├── patterns/                 (QA patterns)
│   └── regression/               (Regression scope notes)
├── instructions/                 (Reference guidance)
│   ├── core/                     (Phase definitions, gates)
│   ├── workflow-rules/           (Spec & diff strategy)
│   ├── qa/                       (Memory & context strategy)
│   ├── token-optimization/       (Token budgets)
│   └── ARCHITECTURE.md           (System design)
└── specs/                        (Feature specifications)
    └── spec-template.md
```

### How to Use QA Workflow

**Step 1: Start QA for a Feature**
```
1. Open QA Workflow/.workflow/workflow-orchestrator.md
2. Go to Phase 1: Requirement QA
3. Ask: "Is this testable? Any ambiguities?"
4. Get REQUIREMENT_APPROVED
```

**Step 2: Plan QA Scope**
```
1. Phase 2: QA Impact Analysis
2. Identify affected modules, test suites, risk areas
3. Get IMPACT_MAPPED
```

**Step 3: Design Tests**
```
1. Phase 3: Spec QA Analysis
2. Use qa-context/test-strategy.md
3. Generate test matrix (functional, API, regression, security)
4. Get SPEC_QA_APPROVED
```

**Step 4: Assess Risk**
```
1. Phase 4: Risk & Threat Analysis
2. Use qa-context/security-model.md
3. Identify vulnerabilities, abuse paths
4. Get RISK_APPROVED
```

**Step 5: Execute & Debug**
```
1. Phase 5: Test Design & Execution
2. Generate and run tests (functional, API, automation)
3. Phase 6: Test Execution & Debug (if failures)
4. Get TEST_RESULTS_READY or FAILURES_TRIAGED
```

**Step 6: Regression & Security**
```
1. Phase 7: Regression Scope (minimal subset from regression-map.md)
2. Phase 8: Security Validation
3. Get REGRESSION_SCOPED and SECURITY_PASS
```

**Step 7: Release Decision**
```
1. Phase 9: Healing Analysis (only if flaky tests)
2. Phase 10: Release Validation (smoke tests + go/no-go)
3. Get RELEASE_APPROVED
```

### Key Features

- ✅ **6 hard approval gates** that cannot be skipped (REQUIREMENT → SPEC → RISK → SECURITY → HEALING → RELEASE)
- ✅ **Persistent QA memory** (bugs, security findings, healing, patterns, regression scopes)
- ✅ **Token-optimized** queries (reference qa-context files, not repeating requirements)
- ✅ **Targeted regression** from regression-map.md (not full test suite reruns)
- ✅ **12 specialized agents** for each phase (deep reasoning)
- ✅ **Diff-focused analysis** (analyze changes, not entire codebase)

### Example: Validating Payment Retry Feature

```
Phase 1: Requirement QA → "Is 'up to 3 times' testable?" → REQUIREMENT_APPROVED
Phase 2: Impact Analysis → "Affects payment-service, webhook, queue" → IMPACT_MAPPED
Phase 3: Spec QA → "Test: success, transient fail, permanent fail, edge cases" → SPEC_QA_APPROVED
Phase 4: Risk → "Abuse: retry bomb, DoS. Mitigated by rate limits" → RISK_APPROVED
Phase 5: Test → "Run functional tests, API contract tests, chaos tests" → TEST_RESULTS_READY
Phase 6: Debug → "All tests pass, no root causes" → FAILURES_TRIAGED
Phase 7: Regression → "Run payment, webhook, queue tests" → REGRESSION_SCOPED
Phase 8: Security → "Auth: existing, RBAC: existing, Secrets: none" → SECURITY_PASS
Phase 9: Healing → "No flaky tests detected" → (Skip)
Phase 10: Release → "Smoke: PASS, Decision: GO" → RELEASE_APPROVED
```

---

## 3️⃣ List of Chat Tools (20+ Platforms)

**Location**: `List of Chat Tools/`

**Purpose**: Practical guides for setup and usage of popular AI chat tools.

### Supported Tools

| Tool | Type | File | Features |
|------|------|------|----------|
| **GitHub Copilot** | IDE extension | [Copilot/](List%20of%20Chat%20Tools/Copilot/) | Summary, Installation, Examples |
| **Claude (Claude.ai)** | Web app | [Claude/](List%20of%20Chat%20Tools/Claude/) | Summary, Installation, Examples |
| **Cursor** | IDE (fork of VS Code) | [Cursor/](List%20of%20Chat%20Tools/Cursor/) | Summary, Installation, Examples |
| **Cline** | IDE extension | [Cline/](List%20of%20Chat%20Tools/Cline/) | Summary |
| **Continue.dev** | IDE extension | [Continue.dev/](List%20of%20Chat%20Tools/Continue.dev/) | Summary |
| **OpenCode** | IDE extension | [OpenCode/](List%20of%20Chat%20Tools/OpenCode/) | Summary, Installation, Examples |
| **Windsurf** | IDE | [Windsurf/](List%20of%20Chat%20Tools/Windsurf/) | Summary |
| **Kiro** | IDE extension | [Kiro/](List%20of%20Chat%20Tools/Kiro/) | Summary, Installation, Examples |
| **Codex** | API | [Codex/](List%20of%20Chat%20Tools/Codex/) | Summary, Installation, Examples |
| **Amazon Q** | AWS service | [Amazon%20Q/](List%20of%20Chat%20Tools/Amazon%20Q/) | Summary |
| **Aider** | CLI tool | [Aider/](List%20of%20Chat%20Tools/Aider/) | Summary |
| **Sourcegraph Cody** | IDE extension | [Sourcegraph%20Cody/](List%20of%20Chat%20Tools/Sourcegraph%20Cody/) | Summary |
| **Tabnine** | IDE extension | [Tabnine/](List%20of%20Chat%20Tools/Tabnine/) | Summary |
| **JetBrains AI** | IDE extension | [JetBrains%20AI%20Assistant/](List%20of%20Chat%20Tools/JetBrains%20AI%20Assistant/) | Summary |
| **Gemini CLI** | Google Gemini | [Gemini%20Cli/](List%20of%20Chat%20Tools/Gemini%20Cli/) | Summary |
| **OpenHands** | Autonomous agent | [OpenHands/](List%20of%20Chat%20Tools/OpenHands/) | Summary |
| **Devin** | Autonomous agent | [Devin/](List%20of%20Chat%20Tools/Devin/) | Summary |
| **(+ more)** | ... | ... | ... |

### What's in Each Tool Folder

Each tool has:
- **Summary.md** — Features, strengths, weaknesses, best for
- **installation_guide.md** — How to install and set up
- **examples.md** — Real usage examples and patterns

### How to Use List of Chat Tools

**Step 1: Choose Your Tool**
```
1. Open List of Chat Tools/README.md
2. Review tool comparison table
3. Read MODEL SELECTION GUIDE.txt for task-model fit
```

**Step 2: Install Your Tool**
```
1. Open List of Chat Tools/[Tool]/installation_guide.md
2. Follow installation steps (varies by tool)
3. Test with a simple prompt
```

**Step 3: Learn Best Practices**
```
1. Read List of Chat Tools/[Tool]/Summary.md
2. Review List of Chat Tools/[Tool]/examples.md
3. Adapt examples to your workflow
```

**Step 4: Integrate with Workflows**
```
1. Use tool with Developer Workflow or QA Workflow
2. Reference: 1. Chat Agent Usage Guide.txt (token tips)
3. Reference: 2. MODEL SELECTION GUIDE.txt (when to use which model)
```

### Recommendations by Use Case

| Use Case | Recommended Tools |
|----------|-------------------|
| **General implementation** | Copilot, Claude, Cursor |
| **Deep analysis & reasoning** | Claude, Cline |
| **Fast, lightweight coding** | Copilot, Cursor |
| **Autonomous multi-step tasks** | Devin, OpenHands, Cline |
| **API-based integration** | Claude (API), Codex |
| **AWS-specific work** | Amazon Q |
| **Search-integrated coding** | Sourcegraph Cody |
| **Git-aware development** | Aider |
| **JetBrains IDEs** | JetBrains AI Assistant |

---

## 📚 Token Optimization and Context Strategy

**Why this matters**: Reusing context and specifications instead of repeating code saves **1000+ tokens per request**, which compounds over time.

### Core Principles

1. **Never repeat requirements** → Use spec paths instead
   ```
   ❌ WRONG: "The payment retry API should retry up to 3 times with exponential backoff..."
   ✅ RIGHT: "Spec: specs/payment-retry.md"
   ```

2. **Never scan full repo** → Use diffs and changed modules
   ```
   ❌ WRONG: "Here's the entire src/ directory: [1000+ lines]"
   ✅ RIGHT: "git diff main..feature -- src/payment/"
   ```

3. **Load context selectively** → Only what you need per phase
   ```
   ❌ WRONG: Load all qa-context files at once
   ✅ RIGHT: Phase 3 loads only qa-context/test-strategy.md
   ```

4. **Reuse persistent memory** → Archive decisions for next time
   ```
   ❌ WRONG: Re-analyze the same bug across sprints
   ✅ RIGHT: Reference qa-memory/bugs/payment-timeout.md
   ```

5. **Compose snippets** → Reuse prompt templates instead of writing from scratch
   ```
   ❌ WRONG: Write a new test generation prompt every time
   ✅ RIGHT: Use snippets/qa/test_generation.snippet.md
   ```

### Token Budgets (by Phase)

**Developer Workflow:**
- Phase 1-2 (Requirement/Impact): 300-400 tokens input
- Phase 3 (Spec Creation): 500-800 tokens input
- Phase 5 (Implementation): 800-1000 tokens input
- Phase 6 (Testing): 600-900 tokens input

**QA Workflow:**
- Phase 1-2 (Requirement/Impact QA): 300-400 tokens input
- Phase 3 (Spec QA): 700 tokens input
- Phase 5 (Test Execution): 800 tokens input
- Phase 8 (Security): 900 tokens input

### Context Files vs Memory

| Type | Write | Read | Purpose | Lifespan |
|------|-------|------|---------|----------|
| **Context** (ai-context/) | Once per release | Often, by all phases | Strategy once, apply always | Long-term (stable) |
| **Memory** (qa-memory/, ai-memory/) | Often, per finding | Selective, per phase | Store decisions for reuse | Medium-term (sprint/project) |

**Example**: 
- **ai-context/test-strategy.md** — "We test all APIs, security, and regression" (write once, reference always)
- **qa-memory/bugs/payment-timeout.md** — "Bug found: transaction timeouts after 30s. Cause: network latency. Fix: increase timeout to 60s" (write once per bug, reference in next similar feature)

---

## 🔄 Recommended Prompt Sequence & How to Run

**Use these prompts in order. Each builds on the previous.**

### Phase 0: Repository Setup (First Time)

**Before running any workflow, prepare your repository.**

#### Step 0a: Build Context
**What**: Create reusable context files for your codebase  
**When**: First time only (or when architecture changes significantly)  
**Run**: `4. prompt to build context.md`  
**Time**: 30-60 minutes

```
1. Open "4. prompt to build context.md"
2. Follow instructions to analyze your codebase
3. Generate ai-context/[feature].md files
4. Review and refine context files
```

#### Step 0b: Merge Context into Workflows
**What**: Integrate context files into all phases  
**When**: After building context (one-time setup)  
**Run**: `5. prompt to merge context.md`  
**Time**: 30-45 minutes

```
1. Open "5. prompt to merge context.md"
2. Select which context files to merge
3. Run prompt to update all workflow phases
4. Verify: Check that phase inputs now reference context files
```

**Result**: Phases now use context files instead of repeating requirements → **Save 1000+ tokens per phase**

### Phase 1: Enable Graph-Based Code Understanding (Optional)

**If your project has graph tools available, enable advanced code analysis.**

#### Step 1a: Incorporate Codeflow Graph (Optional)
**What**: Use CodeFlow snapshot for faster, smarter change analysis  
**Prerequisite**: CodeFlow graph snapshot must exist  
**When**: If available in your environment  
**Run**: `6.1 prompt to incorporate code-flow.txt`  
**Time**: 20-30 minutes

```
1. Check: Does your project have a CodeFlow graph snapshot?
2. If YES, open "6.1 prompt to incorporate code-flow.txt"
3. Run prompt to integrate graph analysis into phases
4. Verify: Phases now use graph for change impact
```

#### Step 1b: Incorporate Code-Review-Graph MCP (Optional)
**What**: Use live code-review-graph MCP for real-time dependency analysis  
**Prerequisite**: code-review-graph MCP must be available  
**When**: If available in your environment  
**Run**: `6.2 prompt to incorporate code-review-graph.txt`  
**Time**: 20-30 minutes

```
1. Check: Is code-review-graph MCP available?
2. If YES, open "6.2 prompt to incorporate code-review-graph.txt"
3. Run prompt to enable MCP tools in phases
4. Verify: Phases can now query dependencies in real-time
```

#### Step 1c: Incorporate Graphify MCP (Optional)
**What**: Use Graphify for knowledge graph queries and pattern discovery  
**Prerequisite**: Graphify MCP must be available  
**When**: If available in your environment  
**Run**: `6.3 prompt to incorporate graphify.txt`  
**Time**: 20-30 minutes

```
1. Check: Is Graphify MCP available?
2. If YES, open "6.3 prompt to incorporate graphify.txt"
3. Run prompt to add graph queries to phases
4. Verify: Phases can now discover patterns via graph
```

### Phase 2: Start Using Workflows

**Now you're ready to use Developer Workflow or QA Workflow.**

#### Developer Workflow
```
1. New feature? Open Developer Workflow/.github/.workflow/workflow-orchestrator.md
2. Start at Phase 1: Requirement Understanding
3. Follow each phase in sequence
4. Stop at gates (SPEC_APPROVED, CODE_APPROVED, RELEASE_READY)
5. Archive decisions to ai-memory/
```

#### QA Workflow
```
1. Testing a feature? Open QA Workflow/.workflow/workflow-orchestrator.md
2. Start at Phase 1: Requirement QA
3. Follow each phase in sequence
4. Stop at gates (REQUIREMENT_APPROVED, SPEC_QA_APPROVED, SECURITY_PASS, RELEASE_APPROVED)
5. Archive findings to qa-memory/
```

### Phase 3: Extend for UI/Frontend (Optional)

**If your project includes UI/frontend work, extend the workflows.**

#### Step 3a: Add UI/UX Testing to QA Workflow (Optional)
**What**: Add UI/UX validation, visual regression, accessibility testing  
**When**: If your project has UI/UX  
**Run**: `7. prompt to update QA workflow for ui-ux testing.md`  
**Time**: 45-60 minutes

```
1. Open "7. prompt to update QA workflow for ui-ux testing.md"
2. Follow instructions to extend QA workflow
3. New phases added: UI Impact, UX Flow, Accessibility, Responsive, Visual Regression, etc.
4. New agents added: Visual QA, Accessibility QA, Responsive QA
5. Archive changes to qa-memory/
```

#### Step 3b: Add Component-Driven Frontend Development to Developer Workflow (Optional)
**What**: Add Figma-driven, component-based, accessibility-aware frontend phases  
**When**: If your project has frontend/UI development  
**Run**: `8. prompt to update developer workflow for UI developer.md`  
**Time**: 45-60 minutes

```
1. Open "8. prompt to update developer workflow for UI developer.md"
2. Follow instructions to extend Developer workflow
3. New phases added: UI/UX Analysis, Figma Analysis, Component Design, Responsive Planning, etc.
4. New agents added: Figma analysis, component mapping, responsive design
5. New snippets added: component generation, design system consistency
6. Archive patterns to ai-memory/
```

---

## ⚠️ Important Precautions & Mandatory Rules

### Rule 1: Never Skip Approval Gates
```
❌ WRONG: "Implement now, spec approval later"
✅ RIGHT: Wait for SPEC_APPROVED, CODE_APPROVED, SECURITY_PASS, RELEASE_APPROVED gates

Impact: Skipping gates leads to rework, security issues, and release delays.
```

### Rule 2: Never Use Full Repo Scans
```
❌ WRONG: Load entire src/ directory: [thousands of lines]
✅ RIGHT: Use git diff and qa-context references

Impact: Saves 600+ tokens per request, improves quality
```

### Rule 3: Never Repeat Requirements in Prompts
```
❌ WRONG: "The payment retry API should retry up to 3 times with exponential backoff..."
✅ RIGHT: "Reference: specs/payment-retry.md"

Impact: Saves 400+ tokens per request, reduces hallucination
```

### Rule 4: Load Context Selectively
```
❌ WRONG: Load all qa-context files at once
✅ RIGHT: 
  - Phase 3: Load qa-context/test-strategy.md
  - Phase 4: Load qa-context/security-model.md
  - Phase 7: Load qa-context/regression-map.md

Impact: Saves 200+ tokens per phase, cleaner outputs
```

### Rule 5: Reuse Hooks, Agents, Snippets
```
❌ WRONG: Write ad-hoc prompts every time
✅ RIGHT: Use existing snippets/hooks/agents from the workflow

Impact: Consistency, quality, saves rewriting time
```

### Rule 6: Archive Every Major Decision
```
❌ WRONG: Analyze the same bug twice in two sprints
✅ RIGHT: Archive to qa-memory/bugs/, qa-memory/patterns/, ai-memory/decisions/

Impact: Cumulative knowledge, faster decisions in future
```

### Rule 7: Sequential Phase Execution
```
❌ WRONG: Run Phase 5 before Phase 3 is approved
✅ RIGHT: Follow the phase dependency graph

Impact: Prevents rework and scope creep
```

### Rule 8: Incremental Updates, Not Rebuilds
```
❌ WRONG: Rewrite entire workflow when something changes
✅ RIGHT: Update the specific instruction/hook/agent that changed

Impact: Maintainability, reduced errors
```

### Rule 9: Never Use Graph Prompts Without Tooling
```
❌ WRONG: Run "6.2 incorporate code-review-graph" without MCP available
✅ RIGHT: Only run graph prompts if corresponding tool is installed

Impact: Prevents errors and wasted time
```

### Rule 10: Keep Workflows Current
```
❌ WRONG: Use old workflow files from 6 months ago
✅ RIGHT: Review and update as team practices evolve

Impact: Workflows stay relevant and effective
```

---

## 💡 Hints & Tips for Success

### General Tips

1. **Start with the orchestrator** — Every workflow starts with `workflow-orchestrator.md`. It's your map.
   
2. **Read the gates** — Before running a phase, check its gate requirements. Gates prevent rework.

3. **Use the template** — Don't write prompts from scratch. Use snippets/hooks/agents provided.

4. **Archive before moving on** — When you finish a phase, save key findings. Your future self will thank you.

5. **Check token usage** — Watch instructions/token-optimization/ for token budgets. Respect them.

6. **Reference, don't repeat** — Paths are your friend. "specs/feature-x.md" saves 400 tokens vs repeating requirements.

7. **Diff-first mindset** — Always ask for diffs first, not full code. Diffs tell the story.

8. **Memory is valuable** — Spend 5 minutes archiving findings. Save 30 minutes later when you need them.

### Developer Workflow Tips

1. **Write the spec first** — A great spec prevents implementation surprises. Take 30-60 min on Phase 3.

2. **Test incrementally** — Don't wait until Phase 6. Test as you implement (Phase 5-6 in parallel).

3. **Reuse test templates** — Use snippets/generate_tests.snippet.md. Don't write tests from scratch.

4. **Keep impact analysis updated** — Phase 2 drives Phase 8 code review scope. Keep it accurate.

5. **Archive decisions** — When you make an architectural choice, save it to ai-memory/decisions/. Next feature author will benefit.

### QA Workflow Tips

1. **Invest in Requirement QA** — 20 min on Phase 1 prevents hours of ambiguity in testing. Worth it.

2. **Use the regression map** — qa-context/regression-map.md is your time saver. It tells you which tests to run.

3. **Threat model early** — Phase 4 (Risk & Threat Analysis) prevents security issues. Don't skip.

4. **Reuse security findings** — qa-memory/security/ is gold. Security patterns repeat. Reference them.

5. **Healing is last resort** — Only use Phase 9 (Healing) if flaky tests are slowing you down. Otherwise, file a bug.

6. **Smoke tests are essential** — Phase 10 smoke validation catches integration issues. Always run.

7. **Archive bug patterns** — qa-memory/bugs/ helps future QA identify similar issues faster.

### Tool Selection Tips

1. **Claude for reasoning** — Use Claude for deep analysis phases (risk, threat modeling, code review).

2. **Copilot for speed** — Use Copilot for quick implementations and incremental work.

3. **Cursor for IDE integration** — Use Cursor for seamless in-IDE development workflow.

4. **Cline for multi-step** — Use Cline for autonomous multi-step tasks (e.g., implement from spec).

5. **Read the tool summary** — Each tool has strengths/weaknesses. Check List of Chat Tools/[Tool]/Summary.md.

### Token Efficiency Tips

1. **Compose snippets** — Use 2-3 snippets together saves more tokens than 1 big prompt.

2. **Reference memory** — "qa-memory/bugs/" costs 2 tokens to reference, 50+ to describe.

3. **Use diffs** — A 50-line diff tells you more than a 1000-line file dump.

4. **Load context selectively** — Phase 3 needs test-strategy.md. Phase 8 needs security-model.md. Don't load both.

5. **Archive early** — The earlier you archive decisions, the more you reuse them.

### Collaboration Tips

1. **Gates are team sync points** — Gates (SPEC_APPROVED, CODE_APPROVED, SECURITY_PASS) are where teams align. Don't bypass.

2. **Memory is team knowledge** — When you archive findings, teammates can reuse them. Invest in good memory notes.

3. **Share context files** — ai-context/ and qa-context/ are team resources. Keep them updated.

4. **Document edge cases** — qa-memory/patterns/ and ai-memory/decisions/ are your team's collective learning.

---

## 🎯 Getting Started (3 Options)

### Option 1: Developer Building a Feature
```
1. Read: Developer Workflow/.github/.workflow/workflow-orchestrator.md
2. Run: 4. prompt to build context.md (first time only)
3. Run: 5. prompt to merge context.md (first time only)
4. Start: Phase 1 (Requirement Understanding)
5. Follow: Sequential phases → gates → archive
```

### Option 2: QA Testing a Release
```
1. Read: QA Workflow/.workflow/workflow-orchestrator.md
2. Read: 1. Chat Agent Usage Guide.txt (token tips)
3. Start: Phase 1 (Requirement QA)
4. Follow: Sequential phases → gates → archive
5. Reference: qa-context/ and qa-memory/ files
```

### Option 3: Team Lead Setting Up
```
1. Read: This README.md (you're reading it!)
2. Review: 2. MODEL SELECTION GUIDE.txt (choose tools)
3. Review: 1. Chat Agent Usage Guide.txt (set expectations)
4. Run: 4. prompt to build context.md with team
5. Run: 5. prompt to merge context.md with team
6. Train: Team on Developer Workflow or QA Workflow orchestrator
7. Go: Phase 1 on first feature
```

---

## 📊 Quick Reference Matrix

| Need | File | Time |
|------|------|------|
| **How to build features** | Developer Workflow/.github/.workflow/workflow-orchestrator.md | 10 min read |
| **How to validate features** | QA Workflow/.workflow/workflow-orchestrator.md | 10 min read |
| **Token efficiency guide** | 1. Chat Agent Usage Guide.txt | 15 min read |
| **Choosing a tool** | 2. MODEL SELECTION GUIDE.txt | 10 min read |
| **Setting up context** | 4. prompt to build context.md | 60 min run |
| **Integrating context** | 5. prompt to merge context.md | 45 min run |
| **Graph integration** | 6.1, 6.2, 6.3 prompts | 30 min each (optional) |
| **UI/UX QA extension** | 7. prompt to update QA workflow for ui-ux testing.md | 60 min run (optional) |
| **Frontend development** | 8. prompt to update developer workflow for UI developer.md | 60 min run (optional) |

---

## 📞 Support & Updates

- **Questions?** Read the orchestrator for your workflow (Developer or QA)
- **Tool issues?** Check List of Chat Tools/[Tool]/installation_guide.md
- **Token problems?** Review 1. Chat Agent Usage Guide.txt and instructions/token-optimization/
- **Updates?** Check this README.md periodically for new prompts and tools

---

**Last Updated**: May 20, 2026  
**Version**: 2.0  
**Status**: Production Ready  
**Workflows**: Developer (10 phases), QA (10 phases), UI/Frontend Extensions Available

## Developer Workflow Overview

Location: Developer Workflow/.github/.workflow/

Core phase flow:

1. Requirement Understanding
2. Impact Analysis
3. Spec Creation (mandatory approval)
4. Design
5. Implementation
6. Testing
7. Debugging
8. Refactoring
9. Code Review
10. Documentation

Execution principle:

- Hook + agent + snippet in small scoped steps
- Diff-focused context
- No implementation before approved spec

### UI/Frontend Extension (prompt 8)

Run prompt 8 to add a frontend development layer on top of the base workflow.

Additional phases:

1. UI/UX Requirement Analysis
2. Figma and Image Analysis
3. Component Impact Analysis
4. Frontend Architecture Planning
5. Design System Mapping
6. Responsive Planning
7. Component Implementation
8. State and Interaction Implementation
9. Accessibility Validation
10. Frontend Testing
11. Visual Review
12. Storybook and Documentation Update
13. PR and UI Review

New assets added by prompt 8: Figma analysis agents, component-focused snippets, design consistency hooks, UI context files, responsive and accessibility workflows.

## QA Workflow Overview

Location: QA Workflow/.qa-workflow/

Core phase flow:

1. Requirement QA
2. QA Impact Analysis
3. Spec QA Analysis
4. Risk and Threat Modeling
5. Test Design
6. Test Execution and Debug
7. Regression Intelligence
8. Security Validation
9. Healing Analysis
10. Release Validation

Execution principle:

- Validation-first with hard release gates
- Targeted regression over broad reruns
- Persistent QA memory for reuse across cycles

### UI/UX QA Extension (prompt 7)

Run prompt 7 to add a dedicated UI/UX validation layer on top of the base QA workflow.

Additional phases:

1. UI Impact Analysis
2. UX Flow Validation
3. Accessibility Review
4. Responsive Validation
5. Interaction Testing
6. Design System Validation
7. Visual Regression Analysis
8. Frontend Performance UX Validation
9. UI Healing
10. Release UX Validation

New assets added by prompt 7: visual QA agents, accessibility and responsive QA agents, UI/UX hooks, UI/UX snippets, visual regression memory, frontend healing workflow with mandatory approval gates.

## Important Instructions

These are mandatory operating rules for this repository.

1. Use prompts in sequence and by requirement.
2. Do not skip prompt 4 or prompt 5 for new repository onboarding.
3. Do not use prompt 6.1, 6.2, or 6.3 unless the corresponding graph capability is available.
4. No implementation without approved spec.
5. Use context files, memory files, and diffs before opening large code areas.
6. Reuse existing hooks, agents, and snippets before writing ad-hoc prompts.
7. Stop at required approval gates before implementation, security actions, healing actions, and release decisions.
8. Keep updates incremental: refine context and memory files instead of rewriting everything.
9. Prefer targeted scans and impacted-file analysis; avoid full-repository rescans during normal task flow.
10. Keep this repository current as workflows evolve.

## Ongoing Update Policy

This repository will continue to receive new changes.

When updating:

- Preserve the phase order and gate model
- Add new prompts/hooks/agents/snippets with clear purpose and placement
- Update workflow summaries when behavior changes
- Record durable patterns and decisions in memory folders

## Quick Start

1. Read the Developer or QA workflow README based on your task type.
2. Execute prompt 4 then prompt 5.
3. Enable graph prompts 6.1 to 6.3 only if corresponding graph tooling is available.
4. Start work from requirement and impact phases, then proceed through gates.
