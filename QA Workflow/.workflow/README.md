# QA Workflow System

**Spec-driven, gate-enforced QA workflow with 10 phases.**

---

## 🚀 START HERE

**[👉 workflow-orchestrator.md](workflow-orchestrator.md)** ← Open this first

Contains:
- All 10 QA phases with execution steps
- Token budgets for each phase
- Approval gates and recovery paths
- What to input, what to output
- Where to archive findings

---

## Quick Navigation

---

## � Documentation

| Need | File |
|------|------|
| **Main entry point** | [workflow-orchestrator.md](workflow-orchestrator.md) |
| **Quick reference** | [workflow.md](workflow.md) |
| **Daily usage** | [daily-usage-guide.md](daily-usage-guide.md) |
| **Phase definitions** | [instructions/core/phases-and-gates.md](instructions/core/phases-and-gates.md) |
| **Approval gates** | [instructions/core/approval-rules.md](instructions/core/approval-rules.md) |
| **System design** | [instructions/ARCHITECTURE.md](instructions/ARCHITECTURE.md) |
| **Spec referencing** | [instructions/workflow-rules/spec-references.md](instructions/workflow-rules/spec-references.md) |
| **Diff analysis** | [instructions/workflow-rules/diff-first-strategy.md](instructions/workflow-rules/diff-first-strategy.md) |
| **Token budgets** | [instructions/token-optimization/token-budgets.md](instructions/token-optimization/token-budgets.md) |
| **Memory strategy** | [instructions/qa/qa-memory-context-strategy.md](instructions/qa/qa-memory-context-strategy.md) |

**[→ Go to workflow-orchestrator.md](workflow-orchestrator.md)**

---

## 📂 Folder Structure

```
.workflow/
├── workflow-orchestrator.md      ← START HERE (main entry point)
├── workflow.md                   (quick reference)
├── daily-usage-guide.md          (fast loop)
│
├── instructions/                 (modular guidance)
│   ├── ARCHITECTURE.md           (system design)
│   ├── README.md                 (instruction overview)
│   ├── core/
│   │   ├── phases-and-gates.md
│   │   └── approval-rules.md
│   ├── workflow-rules/
│   │   ├── spec-references.md
│   │   └── diff-first-strategy.md
│   ├── qa/
│   │   └── qa-memory-context-strategy.md
│   └── token-optimization/
│       └── token-budgets.md
│
├── agents/                       (deep reasoning)
├── hooks/                        (triggered points)
│   ├── HOOK-STANDARD.md
│   ├── CATALOG.md
│   └── [phase-specific hooks]
├── snippets/qa/                  (reusable blocks)
│   ├── SNIPPET-STANDARD.md
│   └── [reusable snippets]
│
├── qa-context/                   (stable strategy)
├── qa-memory/                    (historical findings)
├── specs/                        (feature specs)
└── examples/                     (end-to-end example)
```

---

## ⚡ Quick Start

1. **Open [workflow-orchestrator.md](workflow-orchestrator.md)**
2. **Find your phase (1-10)**
3. **Load inputs** from phase section
4. **Run agent/hook/snippet**
5. **Check gate criteria**
6. **Archive to qa-memory/**
7. **Next phase**

---

## 🔑 Core Concepts

| Concept | See |
|---------|-----|
| **10 phases** | [instructions/core/phases-and-gates.md](instructions/core/phases-and-gates.md) |
| **6 approval gates** | [instructions/core/approval-rules.md](instructions/core/approval-rules.md) |
| **Spec-first rule** | [instructions/workflow-rules/spec-references.md](instructions/workflow-rules/spec-references.md) |
| **Diff-first rule** | [instructions/workflow-rules/diff-first-strategy.md](instructions/workflow-rules/diff-first-strategy.md) |
| **Token budgets** | [instructions/token-optimization/token-budgets.md](instructions/token-optimization/token-budgets.md) |
| **Memory strategy** | [instructions/qa/qa-memory-context-strategy.md](instructions/qa/qa-memory-context-strategy.md) |
| **System design** | [instructions/ARCHITECTURE.md](instructions/ARCHITECTURE.md) |
| **Hook format** | [hooks/HOOK-STANDARD.md](hooks/HOOK-STANDARD.md) |
| **Snippet format** | [snippets/qa/SNIPPET-STANDARD.md](snippets/qa/SNIPPET-STANDARD.md) |

---

## 🎯 Execution by Role

| Role | Start With |
|------|-----------|
| **QA Engineer** | [workflow-orchestrator.md](workflow-orchestrator.md) → [instructions/core/approval-rules.md](instructions/core/approval-rules.md) |
| **Developer** | [daily-usage-guide.md](daily-usage-guide.md) → [instructions/workflow-rules/](instructions/workflow-rules/) |
| **Team Lead** | [instructions/core/phases-and-gates.md](instructions/core/phases-and-gates.md) → [instructions/ARCHITECTURE.md](instructions/ARCHITECTURE.md) |
| **Tool Dev** | [instructions/ARCHITECTURE.md](instructions/ARCHITECTURE.md) → [workflow-orchestrator.md](workflow-orchestrator.md) |

---

**Last updated:** 2026-05-20 | **Status:** Production Ready

**Last updated:** 2026-05-20  
**Status:** Production Ready  
**Version:** 2.0 (Refactored with Modular Instructions)
