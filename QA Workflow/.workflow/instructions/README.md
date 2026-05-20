# Instructions System - Overview

The instructions system provides **modular, reusable guidance** for building QA workflows.

Instead of one giant manual, instructions are organized by topic so you load only what you need.

---

## What's in Here

### Core Instructions
- **[phases-and-gates.md](core/phases-and-gates.md)** — The 10 phases and hard gates that control workflow
- **[approval-rules.md](core/approval-rules.md)** — How approval gates work, evidence requirements

### Workflow Rules
- **[spec-references.md](workflow-rules/spec-references.md)** — How to reference specs in prompts (the #1 rule)
- **[diff-first-strategy.md](workflow-rules/diff-first-strategy.md)** — How to analyze diffs instead of full code

### QA Rules
- **[qa-memory-context-strategy.md](qa/qa-memory-context-strategy.md)** — When to load from `qa-context/` and `qa-memory/`

### Token Optimization
- **[token-budgets.md](token-optimization/token-budgets.md)** — Token limits per phase, allocation strategies

---

## How to Use Instructions

### Quick Answer
**"How do I [do something]?"**

1. Find the relevant instruction file (see table below)
2. Read the section that matches your need
3. Apply the rule

### Building a Prompt
**"I'm building a prompt for Phase 5. What should I include?"**

1. Read: `core/phases-and-gates.md` (what's Phase 5?)
2. Read: `workflow-rules/spec-references.md` (how to reference spec)
3. Read: `workflow-rules/diff-first-strategy.md` (what diff to include)
4. Read: `token-optimization/token-budgets.md` (how many tokens?)
5. Read: `qa/qa-memory-context-strategy.md` (what context to load)
6. Build prompt using all rules

### Understanding Gates
**"What does SPEC_QA_APPROVED mean?"**

Read: `core/approval-rules.md` → Section "SPEC_QA_APPROVED"

### Understanding Phases
**"What's Phase 3?"**

Read: `core/phases-and-gates.md` → Section "Phase 3: Spec QA Analysis"

---

## Instruction Quick Reference

| Need | See |
|------|-----|
| Understand phase definitions | `core/phases-and-gates.md` |
| Understand approval gates | `core/approval-rules.md` |
| Reference specs correctly | `workflow-rules/spec-references.md` |
| Use diffs efficiently | `workflow-rules/diff-first-strategy.md` |
| Load memory and context | `qa/qa-memory-context-strategy.md` |
| Stay within token limits | `token-optimization/token-budgets.md` |
| Understand full architecture | `ARCHITECTURE.md` |

---

## Key Rules (One Per File)

### 🎯 Phases and Gates
**File:** `core/phases-and-gates.md`

Rule: All workflows follow 10 phases with hard gates. You cannot proceed without gate approval.

### ✅ Approval Rules
**File:** `core/approval-rules.md`

Rule: Every gate requires evidence and human approval. Document approval decisions.

### 📋 Spec References
**File:** `workflow-rules/spec-references.md`

Rule: Always reference specs by path (`specs/feature.md`). Never repeat requirements in prompts.

**Impact:** Saves 400+ tokens per prompt

### 📊 Diff-First Strategy
**File:** `workflow-rules/diff-first-strategy.md`

Rule: Analyze changes via diffs, never via full repo scans.

**Impact:** Saves 600+ tokens per analysis

### 🧠 Memory and Context Strategy
**File:** `qa/qa-memory-context-strategy.md`

Rule: Load memory and context selectively by phase. Don't load everything at once.

**Impact:** Saves 200-300 tokens per phase

### 💰 Token Budgets
**File:** `token-optimization/token-budgets.md`

Rule: Each phase has strict input/output token limits. Split phases if over budget.

**Impact:** Keeps prompts focused and efficient

---

## How Instruction Files Relate

```
Core Rules (Phases + Gates)
    ↓
Workflow Rules (How to structure inputs)
    ├─ Spec References
    └─ Diff-First Strategy
    ↓
Execution Rules (Token limits + context)
    ├─ Token Budgets
    └─ Memory/Context Strategy
    ↓
Orchestrator (Master control)
    Uses all instructions to define execution
```

## Using Multiple Instruction Files

Most workflows need multiple instruction files:

**Example: Building Phase 5 Prompt**

```
Orchestrator: workflow-orchestrator.md
  ↓ (tells me Phase 5 inputs/outputs)
  ↓
Instructions to load:
  - core/phases-and-gates.md (what's Phase 5?)
  - workflow-rules/spec-references.md (how to reference)
  - workflow-rules/diff-first-strategy.md (what diff to include)
  - token-optimization/token-budgets.md (token limits)
  - qa/qa-memory-context-strategy.md (what context to load)
  ↓
Apply all rules to build prompt
  ↓
Execute using hooks/snippets
```

---

## Adding New Instructions

When adding a new instruction file:

1. **Choose category**: `core/`, `workflow-rules/`, `qa/`, or `token-optimization/`
2. **Use standard format**: Clear purpose, key rule, examples
3. **Reference in**: `ARCHITECTURE.md` quick reference table
4. **Reference in**: `workflow-orchestrator.md` under "Global Execution Rules" (if globally applicable)
5. **Link from**: Relevant hook/snippet/agent documentation

---

## For Different Audiences

### QA Engineer
Read these to understand gates and approvals:
- `core/approval-rules.md`
- `core/phases-and-gates.md`
- `qa/qa-memory-context-strategy.md`

### Developer (using IDE chat)
Read these to build efficient prompts:
- `workflow-rules/spec-references.md`
- `workflow-rules/diff-first-strategy.md`
- `token-optimization/token-budgets.md`

### Team Lead
Read these to understand governance:
- `core/phases-and-gates.md`
- `core/approval-rules.md`
- `ARCHITECTURE.md`

### Tool Developer
Read these to understand integration:
- `ARCHITECTURE.md`
- `core/phases-and-gates.md`
- `token-optimization/token-budgets.md`

---

## Instruction Philosophy

Instructions should be:
- ✅ **Modular**: Read only what you need
- ✅ **Actionable**: Include examples, not theory
- ✅ **Reusable**: Apply across all phases
- ✅ **Linked**: Reference other instructions when relevant
- ✅ **Tool-agnostic**: No IDE-specific logic
- ✅ **Maintainable**: Single source of truth per concept

---

**Getting started?** Start with `core/phases-and-gates.md`, then `workflow-rules/spec-references.md`.

**Need more?** Read `ARCHITECTURE.md` for how everything fits together.
