# Instruction System Architecture Guide

This document explains how the modular instruction system works together.

## The Three Layers

```
Layer 1: Orchestration
└─ workflow-orchestrator.md (master control)

Layer 2: Instructions
├─ instructions/core/ (phase + gate definitions)
├─ instructions/workflow-rules/ (how to reference specs/diffs/context)
├─ instructions/qa/ (memory and context strategy)
└─ instructions/token-optimization/ (token budgets and efficiency)

Layer 3: Execution
├─ agents/ (deep reasoning for complex phases)
├─ hooks/ (triggered execution points)
└─ snippets/ (reusable prompt blocks)
```

## How It Works

### 1. Start with Orchestrator

When you need to run QA workflow:

```
"What phase am I in?" 
  → Read workflow-orchestrator.md
  → Find phase section
  → Get inputs, outputs, gates
```

### 2. Load Relevant Instructions

Based on phase, load relevant instructions:

```
Phase 5 (Test Design)?
  → Load: instructions/core/phases-and-gates.md (what's the phase)
  → Load: instructions/workflow-rules/spec-references.md (how to reference)
  → Load: instructions/token-optimization/token-budgets.md (token limits)
  → Load: instructions/qa/qa-memory-context-strategy.md (what to load from memory)
```

### 3. Execute Phase

Based on instructions, choose execution path:

```
Instructions → Decision:
  - Simple, structured task? → Use snippet
  - Single phase, gated? → Use hook
  - Deep analysis needed? → Use agent
  - Multi-step workflow? → Use hook chain
```

### 4. Enforce Rules

Instructions define global rules that apply everywhere:

```
Rule 1 (Spec References)
  Source: instructions/workflow-rules/spec-references.md
  Applies to: ALL prompts in ALL phases
  
Rule 2 (Token Budgets)
  Source: instructions/token-optimization/token-budgets.md
  Applies to: ALL hooks and snippets
  
Rule 3 (Approval Gates)
  Source: instructions/core/approval-rules.md
  Applies to: MANDATORY before phase transitions

Rule 4 (Memory Strategy)
  Source: instructions/qa/qa-memory-context-strategy.md
  Applies to: ALL context loading and archival
```

## Instruction Module Descriptions

### Core Instructions (`instructions/core/`)

These define the WHAT (what are the phases and gates):

| File | Content |
|------|---------|
| `phases-and-gates.md` | 10 phases, hard gates, phase dependencies |
| `approval-rules.md` | How approval gates work, evidence requirements |

**Load these when**: You need to understand WHICH gate controls WHAT

### Workflow Rules (`instructions/workflow-rules/`)

These define the HOW (how to structure inputs and context):

| File | Content |
|------|---------|
| `spec-references.md` | How to reference specs (never repeat requirements) |
| `diff-first-strategy.md` | How to use diffs instead of full files |

**Load these when**: You need to structure prompts efficiently

### Token Optimization (`instructions/token-optimization/`)

These define the BUDGET (token limits and efficiency):

| File | Content |
|------|---------|
| `token-budgets.md` | Phase-specific budgets, allocation strategies |

**Load these when**: You're building prompts and need to stay within limits

### QA Instructions (`instructions/qa/`)

These define the MEMORY (how to use context and memory):

| File | Content |
|------|---------|
| `qa-memory-context-strategy.md` | When/what to load from qa-context and qa-memory |

**Load these when**: You're deciding what context to include in prompts

## Execution Components

### Agents (`agents/*.agent.md`)

Agents provide **deep reasoning** for complex phases.

| Agent | Phase | When to Use |
|-------|-------|-----------|
| requirement-qa.agent.md | 1 | Requirement is ambiguous |
| qa-impact.agent.md | 2 | Need deep module impact analysis |
| functional-test.agent.md | 3 | Complex functional scenarios |
| api-test.agent.md | 3 | API contracts to validate |
| risk.agent.md | 4 | Deep risk register building |
| threat-model.agent.md | 4 | Threat modeling needed |
| bug-analysis.agent.md | 6 | Root cause investigation |
| regression.agent.md | 7 | Complex regression scope |
| security.agent.md | 8 | Deep security validation |
| healing.agent.md | 9 | Complex flaky test analysis |
| qa-impact.agent.md | 10 | Release readiness gate |

### Hooks (`hooks/**/*.prompt.md`)

Hooks are **triggered execution points** with gates.

| Hook | Phase | Execution |
|------|-------|-----------|
| post-spec.qa-analysis | 3 | Run after spec available |
| post-spec.risk-analysis | 4 | Run after spec available |
| post-implementation.generate-tests | 5 | Run after code implemented |
| regression-scope-update | 7 | Run after changes scoped |
| pre-release.security-validation | 8 | Run before release |
| post-failure.healing-analysis | 9 | Run if tests flaky |
| pre-release.smoke-validation | 10 | Run before go/no-go |
| release-readiness-review | 10 | Run before go/no-go |

### Snippets (`snippets/qa/*.snippet.md`)

Snippets are **reusable prompt blocks** (no gates, no state).

| Snippet | Use Case |
|---------|----------|
| test_generation.snippet.md | Generate test scenarios |
| api_contract_test.snippet.md | API test design |
| edge_case_generation.snippet.md | Find edge cases |
| regression_analysis.snippet.md | Scope regression tests |
| security_validation.snippet.md | Run security checks |
| healing_analysis.snippet.md | Analyze flaky tests |
| release_validation.snippet.md | Validate release |
| bug_analysis.snippet.md | Root cause analysis |

## Decision Tree: What to Use

```
Decision: What should I run?

├─ Is this a phase transition?
│  └─ YES: Check workflow-orchestrator.md phase section
│          Use hook from that phase
│
├─ Is this a one-time task?
│  └─ YES: Use snippet (no state, no approval needed)
│
├─ Do I need deep reasoning?
│  └─ YES: Use agent (more context, more analysis)
│
├─ Is approval required before proceeding?
│  └─ YES: Use hook (has gate, enforces approval)
│          Read: instructions/core/approval-rules.md
│
└─ Am I composing multiple tasks?
   └─ YES: Use hook chain
           Save intermediate results to qa-memory/
           Load next snippet/hook from prior output
```

## Token Efficiency Path

```
Starting: "I have a complex analysis task"

Step 1: Can I reuse prior analysis?
  → Check qa-memory/ for related findings
  → Load: qa-memory/[category]/related-feature.md
  → Reuse: Saves 200-300 tokens

Step 2: What's the minimal spec reference?
  → Load: specs/feature.md (reference, not full)
  → Load: instructions/workflow-rules/spec-references.md
  → Result: 100 tokens instead of 500

Step 3: What's the minimal context needed?
  → Load: ONE qa-context file only (strategy artifact)
  → Load: instructions/qa/qa-memory-context-strategy.md
  → Result: 200 tokens instead of 1000

Step 4: Build prompt within token budget
  → Load: instructions/token-optimization/token-budgets.md
  → Allocate: Input tokens / Output tokens
  → Check: Am I under budget? If not, split into multiple snippets

Result: Full context with 600 tokens instead of 1500+
```

## Cross-Tool Compatibility

This system is designed to work with:

- GitHub Copilot
- Claude Code
- Cursor
- Cline
- Continue
- OpenCode
- Windsurf
- Any future coding agent

**Key principle**: No tool-specific logic in instructions or orchestrator.

All paths use:
- Standard markdown files
- Workspace-relative paths (no IDE-specific paths)
- Plain English prompts (no JSON, no tool syntax)
- Standard execution patterns (read file → prompt → parse output)

## How to Onboard a New Tool

1. Read `workflow-orchestrator.md` (phases and gates)
2. Read `instructions/core/phases-and-gates.md` (phase definitions)
3. Read `instructions/workflow-rules/spec-references.md` (how to reference)
4. Read `instructions/token-optimization/token-budgets.md` (token limits)
5. Load relevant hooks/snippets per phase
6. Enforce approval gates before transitions
7. Save findings to qa-memory/ (standard paths)

**No tool-specific code needed**. Everything is markdown and plain prompts.

## Maintenance: Keeping Instructions Updated

### When Phase Changes:
1. Update `workflow-orchestrator.md` phase section
2. Update `instructions/core/phases-and-gates.md` (if gate changes)
3. Update relevant hook (if execution changes)
4. Keep approval gates in `instructions/core/approval-rules.md`

### When Token Budgets Change:
1. Update `instructions/token-optimization/token-budgets.md` (source of truth)
2. Update affected hooks/snippets (cross-reference the instruction file)
3. Document reason for budget change

### When New Rule Added:
1. Create instruction file or update existing (in `instructions/`)
2. Reference in `workflow-orchestrator.md` (under "Global Execution Rules")
3. Update relevant hooks/snippets
4. Document in this architecture guide

### When New Hook/Snippet Added:
1. Follow standard format: `HOOK-STANDARD.md` or `SNIPPET-STANDARD.md`
2. Add to appropriate catalog: `hooks/CATALOG.md` or `snippets/qa/README.md`
3. Document cross-references to instructions
4. Ensure no IDE-specific logic

## Quick Reference

| Need | Go To |
|------|-------|
| "What's my phase?" | `workflow-orchestrator.md` |
| "What phase is next?" | `instructions/core/phases-and-gates.md` |
| "What are the gates?" | `instructions/core/approval-rules.md` |
| "What spec format to use?" | `instructions/workflow-rules/spec-references.md` |
| "How to use diffs?" | `instructions/workflow-rules/diff-first-strategy.md` |
| "Token limits?" | `instructions/token-optimization/token-budgets.md` |
| "What to load from memory?" | `instructions/qa/qa-memory-context-strategy.md` |
| "Hook format?" | `hooks/HOOK-STANDARD.md` |
| "Snippet format?" | `snippets/qa/SNIPPET-STANDARD.md` |
| "Hook catalog?" | `hooks/CATALOG.md` |
| "Snippet catalog?" | `snippets/qa/README.md` |

---

**This architecture ensures:**
- ✅ Modular: Reuse instructions across phases
- ✅ Scalable: Add new hooks/snippets without changing core
- ✅ Token-efficient: Clear budgets and context rules
- ✅ Tool-agnostic: Works with any IDE chat agent
- ✅ Maintainable: Single source of truth for each concept
- ✅ Learnable: Clear path from phases → instructions → execution
