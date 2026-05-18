# GitHub Copilot — Usage Examples

**Type:** IDE Extension + Terminal CLI
**Docs:** https://docs.github.com/en/copilot

---

## Basic Usage

```
# Inline suggestion
Start typing a function — Copilot completes it. Press Tab to accept.

# Switch model mid-session (Agent mode)
/model claude-opus-4-6
/model gpt-5
/model gemini-3-pro
```

Inline suggestions work as you type. Chat works in the sidebar panel or inline with `Cmd+I` / `Ctrl+I`.

---

## Code Review

```
# In Copilot Chat
Review this function for edge cases and security issues.
Focus only on the validation logic. Return bullet points only.

# From a PR diff (GitHub.com)
@copilot review this PR — summarise the changes and flag anything risky
```

Copilot reads the diff and returns structured review comments. In Agent mode it can post comments directly to the PR.

---

## Agentic / Multi-file Task

```
# Agent mode (Copilot Chat with Agent mode enabled)
Refactor the authentication module to support OAuth2.
Update all affected files. Show me a plan before making changes.

# Background agent (prefix with &)
& Add unit tests for every public method in src/payments/
```

Agent mode plans the approach, writes code, and verifies across your project. Background agents (`&` prefix) run async while you work.

---

## Tips

- Run `/init` at the start of a new project to generate custom instructions from your codebase
- Use `@workspace` to give Copilot context about your full project, not just the open file
- Use `/model` to switch between Claude, GPT, and Gemini per task without leaving the chat
- Create `.github/copilot-instructions.md` to set persistent rules (coding style, constraints, what to avoid) — Copilot reads this automatically every session
- Use inline chat (`Cmd+I`) for small edits; open the sidebar panel for multi-step tasks

---

## Using Workflow Templates In GitHub Copilot

These workflow templates fit Copilot well because Copilot already supports the three behaviors they rely on:

- persistent repo rules via `.github/copilot-instructions.md`
- repo context via `@workspace`
- multi-file planning and execution via Agent mode

Treat the workflow files as the operating model you point Copilot at, not as a separate extension.

### What To Reference

For implementation work, load the Developer workflow files:

```text
@workspace

Use these files as the workflow source:
- Developer Workflow/.github/.workflow/README.md
- Developer Workflow/.github/.workflow/workflow.md
- Developer Workflow/.github/.workflow/hooks/IDE-EXECUTION-GUIDE.md
```

For validation work, load the QA workflow files:

```text
@workspace

Use these files as the workflow source:
- QA Workflow/.qa-workflow/README.md
- QA Workflow/.qa-workflow/workflow.md
- QA Workflow/.qa-workflow/hooks/IDE-EXECUTION-GUIDE.md
```

### Recommended Copilot Setup

Create a `.github/copilot-instructions.md` file in the target repo and keep it short:

```text
Use the referenced workflow files as the operating model for this repository.

Rules:
- Prefer spec paths, diffs, changed files, and short logs over full file dumps
- When I request a hook, use the referenced hook prompt file exactly
- When I request a chain, guide me step by step and stop at approval gates
- Reuse workflow snippets, hooks, specs, and memory files before inventing new prompts
- Keep outputs scoped to the active workflow phase
```

If you are starting from scratch in a repo, run `/init` first, then tighten the generated instructions to match the workflow above.

### Developer Workflow In Copilot

Use Copilot Chat in Agent mode for most workflow-driven tasks.

```text
@workspace

Use Developer Workflow/.github/.workflow/README.md and
Developer Workflow/.github/.workflow/hooks/IDE-EXECUTION-GUIDE.md
as my development operating model.

When I say "execute hook" or "execute chain", follow those workflow files.
Prefer spec paths, changed files, diffs, and memory files over pasted code.
```

#### Example: Start A New Feature

```text
@workspace

Use Developer Workflow/.github/.workflow/hooks/chains/README.md

Run the "new-feature" chain for "payment-idempotency".
Take me step by step and stop at approval gates.
```

#### Example: Run A Single Hook

```text
@workspace

Use Developer Workflow/.github/.workflow/hooks/implementation/pre-implementation.scope-check.prompt.md

Execute this hook.
Feature: payment-idempotency
Spec: docs/specs/payment-idempotency.md
Target files:
- src/payments/service.ts
- src/payments/controller.ts
- tests/payments/service.test.ts
```

#### Example: Spec-Driven Review

```text
@workspace

Use Developer Workflow/.github/.workflow/hooks/quality/review-diff.prompt.md

Review this diff against the approved spec.
Spec: docs/specs/payment-idempotency.md
Diff: <paste git diff>
Tests: <short test summary>
Return severity-ranked findings only.
```

### QA Workflow In Copilot

Use the QA workflow after implementation, during test design, or before release.

```text
@workspace

Use QA Workflow/.qa-workflow/README.md and
QA Workflow/.qa-workflow/hooks/IDE-EXECUTION-GUIDE.md
as my QA validation operating model.

When I request a hook or chain, follow the referenced prompt files.
Keep outputs findings-first, diff-focused, and scoped to changed modules.
```

#### Example: QA Analysis After Spec

```text
@workspace

Use QA Workflow/.qa-workflow/hooks/spec/post-spec.qa-analysis.prompt.md
and QA Workflow/.qa-workflow/qa-context/test-strategy.md

Execute this hook.
Spec: docs/specs/payment-idempotency.md
Changed modules:
- src/payments/service.ts
- src/payments/controller.ts
```

#### Example: Generate Targeted QA Tests

```text
@workspace

Use QA Workflow/.qa-workflow/hooks/test/post-implementation.generate-tests.prompt.md
and QA Workflow/.qa-workflow/snippets/qa/api_contract_test.snippet.md

Generate targeted QA tests for the approved spec and changed modules only.
Spec: docs/specs/payment-idempotency.md
Diff summary: idempotency key added to create payment flow
```

#### Example: Release Validation

```text
@workspace

Use QA Workflow/.qa-workflow/hooks/release/release-readiness-review.prompt.md

Run release readiness review.
Spec: docs/specs/payment-idempotency.md
Test summary: <short summary>
Open risks: <short list>
```

### Fast Operating Pattern

Use this loop in Copilot for both workflows:

1. Start with `@workspace`
2. Reference the workflow file paths you want Copilot to follow
3. Reference the spec path instead of repeating the requirement
4. Pass only changed files, diff summary, and short logs
5. Run one hook or one chain at a time
6. Save durable outputs into spec, memory, or context files

### When To Use Inline Chat Vs Agent Mode

```text
Use inline chat (Cmd+I / Ctrl+I):
- rewrite a small function
- clean up a test
- make a focused edit under ~20 lines

Use Agent mode:
- execute a workflow chain
- coordinate spec + code + tests
- update multiple files from a hook result
- run review or QA passes across a diff

Use background agents (& prefix):
- generate tests while you continue coding
- run a documentation or changelog pass in parallel
- prepare a review summary while you inspect the diff
```

### Copilot-Specific Best Practices

- Use `@workspace` to ground Copilot in the repo before asking it to follow a workflow
- Keep each request phase-specific; do not combine requirement, implementation, review, and QA in one prompt
- Use `/model` to switch to a stronger model for spec creation, review, debugging, and QA analysis
- Use cheaper/faster models for formatting, short rewrites, and repetitive edits
- If the conversation gets noisy, open a fresh chat and reload only the relevant workflow files
- Keep `.github/copilot-instructions.md` aligned with the workflow so Agent mode does not drift
