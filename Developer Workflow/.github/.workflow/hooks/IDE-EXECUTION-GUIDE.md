# IDE Execution Guide: Running Hooks in Chat Agents

Hooks are copy-paste prompts designed for IDE chat agents. This guide shows exactly how to execute them.

## Prerequisite: Load Hook System

Before first use, point the agent to the hook system:

```
I'm using an AI-assisted development workflow with a hook system.
Reference: .copilot/hooks/

When I request a hook, I'll provide the hook path (e.g., pre-task.context-load).
Use the exact prompt template from that file and execute it.
```

## Workflow 1: Execute a Single Hook

### Step 1: Identify hook
Look at CATALOG.md or chains/README.md to find the right hook.

Example: Ready to start implementation.

### Step 2: Copy-paste hook command
```
Execute hook: hooks/implementation/pre-implementation.scope-check

Feature: payment-idempotency
Spec: specs/payment-idempotency.md
Target files:
  - app/bss_copilot/service.py
  - app/bss_copilot/models.py
  - tests/bss_copilot/test_service.py
```

### Step 3: Agent runs prompt
Agent loads `hooks/implementation/pre-implementation.scope-check.prompt.md` and processes your inputs.

### Step 4: Review output
Agent returns scope validation result.

## Workflow 2: Execute a Hook Chain

### Step 1: Load chain
```
Execute chain: chains/new-feature

Feature: add-payment-idempotency
```

### Step 2: Agent guides you step-by-step
Agent outputs:
```
STEP 1/11: pre-task.context-load
<output>

Ready for STEP 2? (Requirement Agent)
```

### Step 3: Confirm continuation
```
✅ Continue to STEP 2
```

Agent executes next hook automatically.

## IDE-Specific Examples

### GitHub Copilot Chat

**Single hook:**
```
/dev-assist
Execute hook: hooks/implementation/post-implementation.test-generation

Spec: specs/payment-idempotency.md
Changed files: 
  - app/bss_copilot/service.py
  - app/bss_copilot/models.py
Test framework: tests/bss_copilot/
```

**Chain:**
```
/dev-assist
Execute chain: chains/bug-fix
Feature: user-auth-timeout
```

### Claude Code (via @codebase reference)

**Single hook:**
```
@codebase/.copilot

Execute hook: hooks/quality/review-diff

Spec: specs/payment-idempotency.md
Diff: [paste git diff]
Tests: 15/15 passing
```

**Chain:**
```
@codebase/.copilot

Execute chain: chains/new-feature
Feature: add-webhook-retry

Take me through the full workflow.
```

### Cursor (with @files reference)

**Single hook:**
```
@.copilot/hooks/implementation/post-implementation.test-generation

Generate tests for our recent implementation.
Spec: specs/payment-idempotency.md
Changed files: app/bss_copilot/service.py, app/bss_copilot/models.py
```

**Chain:**
```
@.copilot/hooks/chains

Run the "new-feature" chain for "payment-idempotency" feature.
```

## Quick Reference: Common Hook Sequences

### "I just finished coding, what's next?"
```
Execute hooks in sequence:
1. post-implementation.context-update
2. post-implementation.test-generation
3. Run tests
4. pre-review.spec-check
5. quality.review-diff
```

### "I found a bug, help me fix it"
```
Execute hook: hooks/debug/post-debug.memory-archive

Then:
1. Run Debug Agent
2. Apply fix
3. Create regression test
4. post-debug.memory-archive (save bug note)
```

### "Ready to create PR"
```
Execute hook: hooks/pr/pre-pr.gate-readiness

Verify all items checked. Then create PR and run:
post-pr.changelog-update
```

## Best Practices

1. **Always load context first**: Start with `pre-task.context-load`
2. **Never skip gates** (⛔ markers): They prevent mistakes
3. **Save memory notes**: Use `post-*` hooks to avoid re-scanning
4. **Reference, don't repeat**: Link to saved specs/decisions instead of re-explaining
5. **Follow chains**: Use `chains/` when doing multi-step workflows
