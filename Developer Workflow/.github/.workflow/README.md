# AI-Assisted Development System (.github/.workflow)

✅ **CONSOLIDATED & OPTIMIZED** - Single entry point, no duplicate guidance

## 🎯 PRIMARY ENTRY POINT (ONLY)

→ **[workflow-orchestrator.md](workflow-orchestrator.md)** ← START HERE

The orchestrator is the single source of truth for:
- **Workflow routing** (which phase to execute)
- **Phase definitions** (inputs/outputs/gates/constraints)
- **Token budgets** (optimization targets)
- **Gate enforcement** (approval requirements)
- **Task types** (features/bugs/refactors/reviews/hotfixes)

**All other files are supporting assets, not entry points.**

---

## 📚 Supporting Resources (Organized by Role)

### For All Users
| File | Purpose | When to Use |
|------|---------|-------------|
| [daily-usage-guide.md](daily-usage-guide.md) | **Quick practical reference** | Daily workflow checklist |
| [token-optimization-checklist.md](token-optimization-checklist.md) | **Execution checklist** | Before each AI interaction |

### For Implementation
- `agents/` (9 agent definitions) → Use as specified in orchestrator
- `hooks/` (execution triggers) → Execute as specified in orchestrator
- `snippets/` (reusable prompts) → Copy-paste into your workflow
- `specs/` (spec templates) → Use for documentation

### For Understanding
```
instructions/
├─ core/
│  ├─ architecture.md          (System design)
│  ├─ coding-standards.md      (Development standards)
│  ├─ approval-rules.md        (Gate definitions)
│  └─ memory-usage.md          (Persistent memory)
├─ workflow/
│  ├─ lifecycle-routing.md     (10 phases + routing logic)
│  ├─ gate-definitions.md      (Gate mechanics)
│  └─ phase-constraints.md     (Phase budgets/constraints)
├─ token-optimization/
│  └─ context-loading-strategy.md (5 optimization patterns)
└─ developer/
   ├─ implementation-rules.md
   ├─ testing-rules.md
   └─ review-rules.md
```

### For Memory & History
- `ai-memory/` (persistent knowledge base) → Archive decisions/bugs/patterns
- `old/` (deprecated files archive) → Reference only

---

## ❌ Deprecated Files (Archived)

Removed to eliminate duplication:
- ~~`workflow.md`~~ → See [workflow-orchestrator.md](workflow-orchestrator.md)
- ~~`entry-point.prompt.md`~~ → See orchestrator routing section
- ~~`USER-MANUAL.md`~~ → See orchestrator + daily-usage-guide.md
- ~~`hooks/hook-model.md`~~ → See `instructions/workflow/`

Find archived files in [`old/`](old/) folder.

---

## ⚡ Quick Start (30 seconds)

1. **Open**: [workflow-orchestrator.md](workflow-orchestrator.md)
2. **Input your task**: Type, description, spec status, urgency
3. **Follow routing**: Orchestrator tells you which phase to start
4. **Execute**: Use agents/hooks/snippets as directed
5. **Save to memory**: Archive findings in `ai-memory/`

## CORE OPERATING MODEL

**Old**: Start with entry-point prompt, follow hooks, save to ai-memory  
**New**: Start with orchestrator routing, follow phases, load instructions, save to memory/

1. Open [workflow-orchestrator.md](../workflow-orchestrator.md)
2. Describe your task (type + urgency)
3. Orchestrator tells you which phase to execute
4. Load relevant instructions from `instructions/`
5. Execute phase using agent + hooks
6. Save findings to `memory/` (auto-trigger via hooks)
7. Proceed to next phase (orchestrator routing)

## QUICK START (5 MINUTES)

### For Developers Executing Workflows
1. Read [USER-MANUAL.md](../USER-MANUAL.md) (sections 1-3)
2. Open [workflow-orchestrator.md](../workflow-orchestrator.md)
3. Describe your task → Follow routing → Execute phases

### For Tech Leads Managing Adoption
1. Read [MIGRATION-GUIDE.md](../MIGRATION-GUIDE.md)
2. Understand timeline: 2-3 weeks gradual rollout
3. Plan team training + support infrastructure

### For Architects Understanding the System
1. Read [ARCHITECTURE-SUMMARY.md](../ARCHITECTURE-SUMMARY.md)
2. Review `instructions/core/architecture.md`
3. Understand tool generation + modularity

### Reference: Complete File Structure
- **Orchestrator** (central routing): `../workflow-orchestrator.md`
- **Instructions** (reusable guidance): `instructions/` (12+ modules)
- **Agents** (role-based workers): `agents/` (9 agents)
- **Hooks** (lifecycle triggers): `hooks/` (15+ hooks)
- **Snippets** (prompt templates): `snippets/` (7 templates)
- **Specs** (feature specifications): `specs/` (template + approved specs)
- **Memory** (persistent decisions): `memory/` (architecture, workflows, incidents, etc.)
- **User Manual** (complete guide): `../USER-MANUAL.md`
- **Migration Guide** (adoption strategy): `../MIGRATION-GUIDE.md`

## EXAMPLE WORKFLOWS

See [workflow-orchestrator.md](../workflow-orchestrator.md) for workflow routing logic.

### Type A: New Feature (No Spec)
**Duration**: 2-4 hours | **Tokens**: ~4000 | **Phases**: 1-10

→ Follow orchestrator routing → Start Phase 1 (Requirement Agent)

### Type B: Feature (Spec Exists)
**Duration**: 1-3 hours | **Tokens**: ~2000 | **Phases**: 4-10

→ Skip phases 1-3 → Start Phase 4 (Design Agent, optional)

### Type C: Bug Fix
**Duration**: 1-2 hours | **Tokens**: ~2500 | **Phases**: 1,2,3,5-10

→ Follow orchestrator routing → Start Phase 1 (quick requirement analysis)

### Type D: Code Review
**Duration**: 30 minutes | **Tokens**: ~900 | **Phase**: 9 only

→ Jump to Phase 9 (Review Agent) → No implementation needed

### Type E: Hotfix (Emergency)
**Duration**: 30-60 minutes | **Tokens**: ~1500 | **Phases**: 1,2,5-9

→ Spec approval gate BYPASSED (logged) → Fast execution

For detailed examples with inputs/outputs, see [workflow-orchestrator.md](../workflow-orchestrator.md) "Workflow Activation" section.
- debug analysis
- post-debug memory archive
- regression test generation
- review before PR

### 3) Refactor (No Behavior Change)

Goal: Improve structure while preserving behavior.

Prompt:
```text
Execute chain: hooks/chains/refactor
Refactor scope: <module-or-function>
Constraints: behavior must remain unchanged
```

Expected flow:
- impact + scope mapping
- safe implementation
- regression-focused tests
- review for behavior parity

### 4) API Contract Change

Goal: Manage API evolution with break detection and migration notes.

Prompt:
```text
Execute chain: hooks/chains/api-change
Feature: <api-change-name>
Spec path: .copilot/specs/<feature-name>.md
```

Expected flow:
- spec + mandatory approval
- breaking change detection
- migration-aware test generation
- review + PR readiness gate

### 5) Code Review Only

Goal: Validate a diff against spec with severity-ranked findings.

Prompt:
```text
Execute hook: hooks/quality/review-diff
Spec: .copilot/specs/<feature-name>.md
Diff: <git diff>
Tests: <summary>
```

Expected output:
- High/Med/Low findings
- required fixes
- merge readiness yes/no

### 6) Pre-PR Readiness Check

Goal: Ensure no workflow step is skipped.

Prompt:
```text
Execute hook: hooks/pr/pre-pr.gate-readiness
Feature: <feature-name>
Spec: .copilot/specs/<feature-name>.md
```

Expected output:
- pass/fail checklist
- missing items to complete before PR

## Daily Copilot Routine (Practical)

1. Run `hooks/pre-task/context-load`.
2. Reference approved spec path (`.copilot/specs/...`) for all implementation prompts.
3. Use only changed files/diffs for coding and review tasks.
4. Run post-hooks to persist decisions, patterns, and bugs.
5. Run `hooks/pr/pre-pr.gate-readiness` before opening PR.

## Token-First Rules

- Prefer file paths + diff summaries over full file content.
- Reuse snippets from `snippets/` instead of writing long custom prompts.
- Reference saved memory in `ai-memory/` instead of repeating history.
- Keep prompts task-scoped (function/module level), not repo-scoped.
- Treat specs as the single source of truth.

## Operational Rules

- No implementation before an approved spec.
- Pass only minimum required context per phase.
- Reuse snippets; do not rewrite long prompts.
- Keep chat context short; persist reusable knowledge in files.
- Use gates to stop and request confirmation at critical points.

---

## Entry Point System (Recommended)

The **entry point** automatically routes every new requirement to the correct workflow step based on:
- Whether a spec exists and is approved
- What type of work is needed (feature, bug, review, refactor)
- Whether the scope is clear

### Quick Start

```
Execute entry point:

Requirement: <describe what you need>
Spec path: <specs/name.md or leave blank>
Affected files: <optional list>
```

### Three Outcomes

1. **Spec exists + approved** → Routed to implementation (scope check → code → tests → review)
2. **Spec exists + pending** → Routed to approval gate
3. **Spec missing** → Routed to auto-create minimal spec OR direct fix (for bugs)

### Files

- `entry-point.prompt.md` — Full entry point implementation
- `ENTRY-POINT-QUICKSTART.md` — Visual examples and decision flows
- `../copilot-instructions.md` — Entry point integrated into Copilot CLI

### When to Use

**Use entry point for:**
- Every new task (feature, bug, review, refactor)
- Continuation of previous work (spec already exists)
- Multi-person projects (spec as shared contract)

**Skip entry point only for:**
- Trivial 1-line fixes (comments, typos)
- Documentation-only changes (rare)
- Even then, using entry point is still recommended

### Token Savings

- Manual workflow selection: 10-15 min, 500+ tokens
- Entry point automation: 2-3 min, 200-300 tokens (routing only)
- **Overall savings: ~50% per task when combined with spec reuse**

---

## File Organization

```
.github/
  ├── copilot-instructions.md          ← Copilot CLI (references entry point)
  └── .workflow/
      ├── README.md                    ← You are here
      ├── workflow.md                  ← 10-phase workflow definition
      ├── entry-point.prompt.md        ← Smart requirement router
      ├── ENTRY-POINT-QUICKSTART.md    ← Entry point visual guide
      ├── daily-usage-guide.md         ← Day-to-day patterns
      ├── token-optimization-checklist.md
      ├── hooks/
      │   ├── CATALOG.md
      │   ├── pre-task/
      │   ├── implementation/
      │   ├── quality/
      │   ├── review/
      │   ├── debug/
      │   ├── pr/
      │   └── chains/
      ├── agents/
      │   ├── requirement.agent.md
      │   ├── spec.agent.md
      │   ├── implementation.agent.md
      │   ├── test.agent.md
      │   └── ...
      ├── snippets/
      │   ├── review_diff.snippet.md
      │   ├── test_generation.snippet.md
      │   └── ...
      ├── specs/
      │   └── (approved feature specs)
      └── ai-memory/
          └── (persistent memory files)
```

---

## Quick Links

| Need | Go To |
|------|-------|
| Start a new task | `entry-point.prompt.md` or `../copilot-instructions.md` |
| Understand the workflow | `workflow.md` |
| Visual entry point examples | `ENTRY-POINT-QUICKSTART.md` |
| Day-to-day patterns | `daily-usage-guide.md` |
| Available hooks | `hooks/CATALOG.md` |
| Copy-paste snippets | `snippets/` |
| Role-based AI agents | `agents/` |
| Approved specs | `specs/` |
| Long-term memory | `ai-memory/` |
