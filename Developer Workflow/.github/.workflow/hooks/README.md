# Hook System: Master Index

The hook system is a library of copy-paste-ready prompts that automate context management, prevent re-scanning, and enforce gated workflows inside IDE chat agents.

## What Are Hooks?

Hooks are **prompt templates** designed to be executed sequentially or in chains. They:
- Reduce token usage by 50-60%
- Prevent context loss between chat sessions
- Enforce spec-driven development gates
- Automate context file updates
- Are 100% IDE-agnostic (work in any chat agent)

## Quick Start (2 minutes)

1. Read [CATALOG.md](CATALOG.md) to understand all available hooks.
2. Pick a workflow from [chains/README.md](chains/README.md).
3. Follow [IDE-EXECUTION-GUIDE.md](IDE-EXECUTION-GUIDE.md) for your IDE.
4. Use [DAILY-USAGE-EXAMPLES.md](DAILY-USAGE-EXAMPLES.md) for copy-paste commands.

## Hook Types (By Purpose)

### Pre-Task Hooks (Context Loading)
- `pre-task/context-load`: Load specs, decisions, patterns

### Workflow Hooks (Main Development Phases)

**Spec Phase:**
- `spec/post-requirement.auto-create-spec`: Auto-create draft spec from requirement + impact
- `spec/approval-gate`: Gate implementation on spec approval

**Implementation Phase:**
- `implementation/pre-implementation.scope-check`: Validate scope
- `implementation/post-implementation.context-update`: Save decision
- `implementation/post-implementation.test-generation`: Auto-generate tests

**Review Phase:**
- `review/pre-review.spec-check`: Validate diff aligns with spec
- `review/post-review.findings-archive`: Save review findings

**Quality Hooks:**
- `quality/review-diff`: Perform focused code review
- `quality/detect-breaking-changes`: Flag API breaks

**Debug Phase:**
- `debug/post-debug.memory-archive`: Save bug root cause

**PR Phase:**
- `pr/pre-pr.gate-readiness`: Verify PR readiness
- `pr/post-pr.changelog-update`: Auto-update changelog

**Context Maintenance:**
- `context/update-patterns`: Extract and save patterns

### Memory Hooks (Context Saving)
- `post-impact/memory-snapshot`: Save impact analysis
- `post-implementation.context-update`: Save implementation decision
- `post-review.findings-archive`: Save review findings
- `post-debug.memory-archive`: Save bug fix
- `post-pr.changelog-update`: Update changelog
- `context.update-patterns`: Save new pattern

## File Structure

```
.copilot/hooks/
├── CATALOG.md                          # All hooks listed
├── DEPENDENCY-GRAPH.md                 # Hook connections & rules
├── IDE-EXECUTION-GUIDE.md              # How to use in each IDE
├── TOKEN-OPTIMIZATION-STRATEGY.md      # Token budgets & savings
├── DAILY-USAGE-EXAMPLES.md             # Real scenarios
│
├── pre-task/
│   └── context-load.prompt.md          # Load context before starting
│
├── implementation/
│   ├── pre-implementation.scope-check.prompt.md
│   ├── post-implementation.context-update.prompt.md
│   └── post-implementation.test-generation.prompt.md
│
├── spec/
│   ├── post-requirement.auto-create-spec.prompt.md
│   └── approval-gate.prompt.md         # Gate on spec approval
│
├── debug/
│   └── post-debug.memory-archive.prompt.md
│
├── review/
│   ├── pre-review.spec-check.prompt.md
│   └── post-review.findings-archive.prompt.md
│
├── quality/
│   ├── review-diff.prompt.md
│   ├── detect-breaking-changes.prompt.md
│   └── update-patterns.prompt.md       # Save patterns discovered
│
├── context/
│   └── update-patterns.prompt.md
│
├── pr/
│   ├── pre-pr.gate-readiness.prompt.md
│   └── post-pr.changelog-update.prompt.md
│
└── chains/
    └── README.md                       # Multi-step workflow chains
```

## How to Execute a Hook

### In Chat (Copy-Paste Method)

```
Execute hook: hooks/implementation/post-implementation.test-generation

Spec: specs/payment-idempotency.md
Changed files: app/bss_copilot/service.py
Test framework: tests/bss_copilot/
```

Agent loads the prompt template and executes.

### Using a Hook Chain

```
Execute chain: chains/new-feature
Feature: payment-idempotency

Agent guides you through 11 steps automatically.
```

## Key Concepts

### Spec-Driven Development
Every hook references a spec file instead of re-explaining requirements. Specs are single source of truth.

### Memory System
Post-hooks save decisions to `ai-memory/` so future tasks load context instead of rescanning.

### Gating
Critical gates (approval-gate, pre-pr.gate-readiness) block proceeding until conditions met.

### Token Budgets
Each hook has strict token limits to prevent context bloat. See TOKEN-OPTIMIZATION-STRATEGY.md.

### No Automation Code
Hooks are prompts, not scripts. Execute them manually in chat—no CI/CD setup required.

## Recommended First Use

1. Start with [DAILY-USAGE-EXAMPLES.md](DAILY-USAGE-EXAMPLES.md) → pick a scenario
2. Follow the hook chain step-by-step
3. Let agent guide you
4. Reference memory notes instead of re-explaining

## Common Questions

**Q: Do I need to execute hooks in a specific order?**  
A: Yes. Follow the dependency graph in DEPENDENCY-GRAPH.md. Use chains for automatic sequencing.

**Q: Can I skip a hook?**  
A: Gates (⛔) cannot be skipped. Optional hooks can be skipped, but memory notes won't be saved.

**Q: What IDE works best?**  
A: Any IDE with chat (Copilot, Claude, Cursor, Continue.dev). See IDE-EXECUTION-GUIDE.md.

**Q: How much do hooks save in tokens?**  
A: 50-60% reduction vs. ad-hoc prompting. See TOKEN-OPTIMIZATION-STRATEGY.md for details.

**Q: Can I add custom hooks?**  
A: Yes. Follow the prompt template format and add to appropriate folder.

## Next Steps

1. **Quick Dive**: Read [DAILY-USAGE-EXAMPLES.md](DAILY-USAGE-EXAMPLES.md)
2. **Full Reference**: Read [CATALOG.md](CATALOG.md)
3. **IDE Setup**: Follow [IDE-EXECUTION-GUIDE.md](IDE-EXECUTION-GUIDE.md)
4. **Token Discipline**: Review [TOKEN-OPTIMIZATION-STRATEGY.md](TOKEN-OPTIMIZATION-STRATEGY.md)
5. **Chains**: Explore [chains/README.md](chains/README.md) for complete workflows

---

**System Status**: Complete ✅
- 15 core hooks defined
- 4 multi-step chains defined
- IDE execution guide included
- Token optimization rules documented
- Daily usage examples provided
