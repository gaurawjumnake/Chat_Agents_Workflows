# Shared Architecture Principles

## Design Philosophy

This workflow system is built on **modular, instruction-driven design**. Every component is:

- **Tool-agnostic**: Runs on Copilot, Claude, Cursor, Cline, Continue, Windsurf, etc.
- **Phase-gated**: Hard stops at critical approval points
- **Memory-persistent**: Decisions and context are stored for reuse across sessions
- **Token-efficient**: Minimizes repeated scanning and context reloads
- **Spec-driven**: Single source of truth for each feature/bug/refactor
- **Incremental**: Small focused prompts, not massive context dumps

## System Components

### 1. **Workflow Orchestrator**
Central routing logic that determines:
- Which phase to execute next
- Which agent/hook to invoke
- Which context/memory to load
- Which gates apply

Entry point: See `workflow-orchestrator.md`

### 2. **Instructions** (/instructions/)
Reusable guidance organized by domain:
- **core/**: Universal rules (approval, memory, standards)
- **workflow/**: Phase logic and gate definitions
- **developer/**: Implementation, testing, review rules
- **token-optimization/**: Context loading, budget enforcement
- **security/**: Security review, data handling rules

Each instruction is:
- Modular (can load independently)
- Self-contained (no forward references)
- Tool-portable (no tool-specific syntax)
- Reference-friendly (easy to cite in prompts)

### 3. **Agents** (/agents/)
Lightweight role-based workers (50-100 lines each):
- Requirement agent → Analyzes input
- Impact agent → Traces affected files
- Spec agent → Drafts specification
- Design agent → Plans architecture
- Implementation agent → Generates code
- Test agent → Creates tests
- Debug agent → Diagnoses issues
- Review agent → Performs code review
- Documentation agent → Updates docs

Each agent:
- Has a single, narrow responsibility
- Accepts minimal input (spec path + focused files only)
- Generates focused output (not full repo rewrites)
- Respects token budgets (500-1200 tokens per agent)

### 4. **Hooks** (/hooks/)
Pre/post-workflow execution points:
- **Pre-task**: Load context, memory, specs
- **Spec phase**: Draft spec, approval gate
- **Implementation**: Scope check, archival, test generation
- **Review**: Spec alignment check, detailed review, breaking change detection
- **Quality**: Risk analysis, security checks
- **PR**: Readiness gate, changelog update

Hooks are:
- Optional but repeatable
- Chainable (one hook output → next hook input)
- Gate-aware (can block execution)
- Memory-integrated (save findings for reuse)

### 5. **Snippets** (/snippets/)
Reusable prompt templates organized by domain:
- **developer/**: code review, testing, implementation prompts
- **ui/**: design review, accessibility prompts
- **security/**: threat analysis, data handling prompts

Snippets:
- Are reference-only (copy-paste into prompts)
- Avoid full repo context
- Support composition (combine multiple snippets)
- Reference relevant specs/agents

### 6. **Memory System** (/memory/)
Persistent decision storage across sessions:
- **architecture/**: Design decisions, system maps
- **workflows/**: Workflow patterns, optimization notes
- **incidents/**: Bug analysis, root causes
- **ui/**: Design patterns, accessibility decisions
- **security/**: Security decisions, audit notes
- **patterns/**: Code patterns, idioms
- **regression/**: Test failure analysis, fixes

Memory is:
- Searchable by session/date
- Referenced via path (not full text)
- Updated incrementally (never full rewrites)
- Loaded selectively (only relevant notes)

### 7. **Specifications** (/specs/)
Single source of truth for features/bugs:
- One spec file per feature/issue
- Locked after approval (no mid-flight changes)
- Referenced in all implementation/test/review prompts
- Archive-friendly (searchable by date/keyword)

## Execution Model

```
User Input
    ↓
Load Orchestrator Rules
    ↓
Determine Phase + Next Step
    ↓
Load Relevant Instructions
    ↓
Load Relevant Memory/Context
    ↓
Invoke Agent OR Hook
    ↓
Generate Output
    ↓
Save to Memory (if needed)
    ↓
Gate Check (if applicable)
    ↓
Next Phase OR Complete
```

## Tool Portability

All instructions, agents, and hooks are **tool-agnostic**:

- No tool-specific syntax (no Copilot-only features)
- No IDE-specific commands (no JetBrains-only shortcuts)
- No vendor lock-in (can switch tools without rewriting)

Tool-specific layers:
- **copilot-instructions.md** (generated from orchestrator)
- **.cursorrules** (generated from orchestrator)
- **claude.md** (generated from orchestrator)
- etc.

These are **generated** from shared instructions, never manually edited.

## Constraints & Assumptions

1. **Spec-first development**: No code without approved spec
2. **Minimal context**: Pass only what's needed; let workflows load the rest
3. **Memory-persistent**: Decisions are stored once and referenced thereafter
4. **Phase-gated**: Approval gates cannot be bypassed
5. **Token-budgeted**: Each agent has token limits (see token-optimization/)
6. **Incremental updates**: Never rewrite memory/context; always append/update
7. **Tool-neutral**: No vendor-specific features
8. **Session-aware**: Each session loads relevant memory from previous sessions

## Integration with QA Workflows

The Developer Workflow and QA Workflow are parallel but independent:
- Developer Workflow: Code change lifecycle
- QA Workflow: Testing and validation lifecycle

Integration points:
- Specs link to test cases (in QA Workflow)
- Post-implementation tests feed QA gates
- PR gates check QA approval status

Both workflows reference shared Memory system.
