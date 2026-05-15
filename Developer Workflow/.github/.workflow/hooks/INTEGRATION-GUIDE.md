# Hook System Integration & Setup Guide

This document ties the hook system to the overall Spec-DD workflow and shows how all .copilot components work together.

## System Architecture

```
.copilot/
  ├── README.md (START HERE)
  ├── workflow.md (Overall workflow phases)
  ├── daily-usage-guide.md (Day-to-day operation)
  │
  ├── /agents/ (Lightweight agent roles)
  │   └── requirement, impact, spec, design, implementation, test, debug, review, documentation
  │
  ├── /snippets/ (Reusable prompt templates)
  │   └── explain_code, impact_analysis, create_spec, implement_from_spec, etc.
  │
  ├── /hooks/ (AUTO-TRIGGER PROMPTS) ← YOU ARE HERE
  │   ├── README.md (Hook index)
  │   ├── CATALOG.md (All hooks listed)
  │   ├── DEPENDENCY-GRAPH.md (Hook connections)
  │   ├── IDE-EXECUTION-GUIDE.md (IDE setup)
  │   ├── TOKEN-OPTIMIZATION-STRATEGY.md (Token rules)
  │   ├── DAILY-USAGE-EXAMPLES.md (Real scenarios)
  │   │
  │   ├── /pre-task/ (Context loading)
  │   ├── /implementation/ (Coding + testing)
  │   ├── /spec/ (Spec approval)
  │   ├── /review/ (Code review)
  │   ├── /quality/ (Testing & breaking changes)
  │   ├── /debug/ (Bug fixing)
  │   ├── /pr/ (PR gates & changelog)
  │   ├── /context/ (Memory updates)
  │   └── /chains/ (Multi-step workflows)
  │
  ├── /specs/ (Approved feature specs)
  │   ├── spec-template.md
  │   └── README.md
  │
  ├── /ai-memory/ (Persistent memory: Obsidian-compatible)
  │   ├── /specs/ (spec summaries)
  │   ├── /decisions/ (architectural decisions)
  │   ├── /patterns/ (reusable patterns)
  │   ├── /bugs/ (root causes + fixes)
  │   └── /snippets/ (snippet notes)
  │
  └── /examples/ (end-to-end-flow.md)
```

## How Hooks Fit In

### The Old (Inefficient) Way
```
Chat Session 1:
- "What's the spec?"
- [Restate requirement]
- "Okay, impact analysis..."
- [Re-explain what changed]
- [Full code context loaded]

Chat Session 2 (next day):
- "What did we decide?"
- [Re-explain all decisions]
- [Reload all context]
→ Wastes ~4000 tokens on context reload
```

### The New (Hook-Driven) Way
```
Chat Session 1:
- Execute hook: pre-task/context-load
  [Agent loads spec + decisions from files: 200 tokens]
- Execute hook: implementation agents
  [Reference spec by path only: minimal context]
- Execute hook: post-implementation/context-update
  [Save decision to ai-memory/: 100 tokens]

Chat Session 2 (next day):
- Execute hook: pre-task/context-load
  [Load saved decision from yesterday: 150 tokens]
- Continue work with context already loaded
→ Saves 3700+ tokens
```

## Integration With Workflow Phases

Hooks execute during workflow phases:

| Phase | Involved Hooks |
|---|---|
| 1. Requirement | pre-task.context-load |
| 2. Impact | post-impact.memory-snapshot |
| 3. Spec | post-spec.approval-gate |
| 4. Design | (no hooks; Design Agent only) |
| 5. Implementation | pre-implementation.scope-check, post-implementation.* |
| 6. Testing | post-implementation.test-generation |
| 7. Debugging | post-debug.memory-archive |
| 8. Refactoring | context.update-patterns (if needed) |
| 9. Code Review | pre-review.spec-check, quality.review-diff |
| 10. Documentation | (no hooks; Documentation Agent only) |

## Integration With Agents

Hooks work WITH agents, not instead of them:

```
pre-task/context-load
  ↓
[Agent uses loaded context]
  ↓
Requirement Agent
  ↓
post-impact/memory-snapshot
  ↓
Impact Agent
  ↓
[etc.]
```

**Agent:** Does the work (thinking, generating, analyzing)
**Hooks:** Automate context loading & memory saving

## Integration With Specs & Memory

Hooks connect specs to memory:

```
Spec Created (specs/feature.md)
  ↓
post-spec.approval-gate
  ↓
Implementation starts
  ↓
post-implementation.context-update
  (Saves decision to ai-memory/decisions/)
  ↓
Next task:
pre-task.context-load
  (Loads spec + decision from memory)
  ↓
No re-explaining needed ✅
```

## Setup Checklist

### 1. First Time Using Hooks

- [ ] Read `.copilot/hooks/README.md` (this folder's index)
- [ ] Read `.copilot/hooks/CATALOG.md` (all available hooks)
- [ ] Pick a workflow from `.copilot/hooks/chains/README.md`
- [ ] Read `.copilot/hooks/IDE-EXECUTION-GUIDE.md` for your IDE

### 2. First Spec & Implementation

- [ ] Create spec in `specs/feature-name.md`
- [ ] Execute hook: `hooks/spec/approval-gate`
- [ ] Approve spec (gate enforces this)
- [ ] Execute hook: `hooks/implementation/pre-implementation.scope-check`
- [ ] Implement with Implementation Agent
- [ ] Execute hook: `hooks/post-implementation/context-update`
- [ ] Execute hook: `hooks/post-implementation/test-generation`
- [ ] Execute tests
- [ ] Execute hook: `hooks/review/pre-review.spec-check`
- [ ] Execute hook: `hooks/quality/review-diff`
- [ ] Execute hook: `hooks/pr/pre-pr.gate-readiness`
- [ ] Create PR
- [ ] Execute hook: `hooks/post-pr/changelog-update`

### 3. Enable Muscle Memory

- [ ] Save the 3 most common chains from `.copilot/hooks/chains/` as bookmarks
- [ ] Bookmark `.copilot/hooks/DAILY-USAGE-EXAMPLES.md`
- [ ] Bookmark `.copilot/hooks/IDE-EXECUTION-GUIDE.md` section for your IDE

### 4. Team Adoption (Optional)

- [ ] Share `.copilot/hooks/README.md` with team
- [ ] Standard: All features require approved spec (enforced by gate)
- [ ] Standard: All PRs must pass hooks before creation
- [ ] Document team's hook customizations in `.copilot/ai-memory/patterns/`

## Quick Reference: What Hook Do I Need?

### "I'm starting a task"
→ `hooks/pre-task/context-load`

### "I just defined the scope, what changes?"
→ `hooks/post-impact/memory-snapshot`

### "I wrote a spec, ready to code?"
→ `hooks/spec/approval-gate` ⛔

### "I finished coding, now what?"
→ `hooks/post-implementation/context-update` + `test-generation`

### "Before I submit a PR..."
→ `hooks/review/pre-review.spec-check` + `quality.review-diff` + `pr/pre-pr.gate-readiness`

### "I found a bug"
→ `hooks/debug/post-debug.memory-archive`

### "I discovered a pattern"
→ `hooks/context/update-patterns`

## Daily Workflow (Spec-DD + Hooks)

```
Morning:
1. Task: "Build feature X"
2. Run: hooks/pre-task/context-load → loads all context ✅
3. Run: Requirement Agent → creates summary
4. Run: Impact Agent → finds affected files
5. Run: hooks/post-impact/memory-snapshot → saves analysis

Midday:
6. Run: Spec Agent → writes spec
7. Run: hooks/spec/approval-gate ⛔ (wait for approval)
8. [Approval received]
9. Run: hooks/implementation/pre-implementation.scope-check
10. Run: Implementation Agent → writes code
11. Run: hooks/post-implementation/context-update → saves decision

Afternoon:
12. Run: hooks/post-implementation/test-generation → auto-generate tests
13. Run: Test Agent (execute generated tests)
14. Run: hooks/review/pre-review.spec-check → validate alignment
15. Run: hooks/quality/review-diff → review code
16. Run: hooks/pr/pre-pr.gate-readiness ⛔ (ready?)
17. [All checks pass]
18. Create PR
19. Run: hooks/post-pr/changelog-update

Next day:
20. Task: "Build related feature Y"
21. Run: hooks/pre-task/context-load
    [Agent loads saved decisions + patterns from yesterday]
    [No re-explaining needed]
→ Saves 1000+ tokens vs. ad-hoc workflow
```

## Token Tracking

Use this table to track your token usage:

| Step | Hook/Agent | Input Tokens | Output Tokens | Saved Tokens |
|---|---|---|---|---|
| 1 | pre-task.context-load | 300 | 200 | 700 (vs. full context) |
| 2 | Requirement Agent | 400 | 200 | - |
| 3 | Impact Agent | 500 | 150 | 600 (vs. full scanning) |
| 4 | post-impact | 400 | 150 | 400 (saved to memory) |
| 5 | Spec Agent | 600 | 500 | - |
| 6 | post-spec gate | 200 | 100 | - |
| 7 | Implementation | 700 | 600 | - |
| 8 | post-impl update | 500 | 200 | 300 (saved) |
| 9 | test-generation | 700 | 400 | - |
| **Total** | - | **4800** | **2500** | **2000** |

**vs. Ad-Hoc Workflow**: ~7000 tokens for same work
**Savings: 28%** from hooks alone; **50% more** if ai-memory prevents re-scanning

## Troubleshooting

### "Hook output is longer than expected"
→ Check TOKEN-OPTIMIZATION-STRATEGY.md for budget limits. Split into sub-hooks if needed.

### "I can't find relevant context when loading"
→ Run `post-*` hooks to save decisions after each phase. Tomorrow's `pre-task` will find them.

### "A gate is blocking me"
→ Gates exist for a reason. Resolve the gate condition:
- Spec gate: Get spec approved
- PR gate: Fix any failing checks

### "I'm using too many tokens"
→ Read TOKEN-OPTIMIZATION-STRATEGY.md. Likely issue: pasting full files instead of diffs.

## Next Steps

1. **Right Now**: Read `.copilot/hooks/DAILY-USAGE-EXAMPLES.md`
2. **First Feature**: Follow `chains/new-feature` step-by-step
3. **Team Adoption**: Share `.copilot/hooks/README.md` with team
4. **Customize**: Add custom hooks to `.copilot/hooks/` as needed

---

**System Complete** ✅

- 15 core hooks defined and copy-paste ready
- 4 multi-step chains for common workflows
- IDE execution guides for Copilot, Claude, Cursor
- Token optimization rules documented
- Daily usage examples provided
- Integration guide included
