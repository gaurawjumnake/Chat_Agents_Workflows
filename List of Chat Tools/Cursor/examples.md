# Cursor — Usage Examples

**Type:** AI-native IDE (VS Code fork)
**Docs:** https://cursor.com/en/docs

---

## Basic Usage

```
# Inline completion
Start typing — Cursor auto-completes as you write. Press Tab to accept.

# Inline edit (single file)
Select code → Cmd+K (Mac) / Ctrl+K (Windows/Linux)
> "Rewrite this to use async/await"

# Switch model for current task
Cmd+Shift+P → "Switch Model" → select Claude Opus / GPT-5 / Gemini
```

Cursor auto-completes and suggests as you type. Use `Cmd+K` for quick inline edits on a selection.

---

## Code Review

```
# Open Composer (multi-file chat)
Cmd+I → type your review request

> Review the changes I made in the last commit.
  Focus on the auth module. Return bullet points only.

# Reference a specific file
> @auth/login.py — is there a race condition in the token refresh logic?
```

Use `@filename` to reference any file. Composer reads it without you pasting the contents.

---

## Agentic / Multi-file Task

```
# Composer — multi-file task
Cmd+I → Agent mode ON

> Refactor the payment service to separate concerns:
  - Move validation to validators.py
  - Move DB calls to repository.py
  - Update all imports across the project

# Parallel agents (up to 8)
Open multiple Composer windows → run different tasks simultaneously
```

Agent mode plans the change, edits files across the repo, and shows a diff for review before applying.

---

## Tips

- Use `Cmd+I` (inline) for edits under 20 lines — it's faster and cheaper than Composer
- Use Composer (`Cmd+I` in Agent mode) for anything spanning multiple files
- Add a `.cursorrules` file at your repo root to set persistent instructions — Cursor reads it every session
- Reference files with `@filename` instead of pasting code — saves tokens and keeps prompts short
- Use `Cmd+Shift+J` to open Cursor Settings and switch the default model per task type (cheap model for completions, frontier model for Agent mode)

---

## Using Workflow Templates In Cursor

These workflow templates work well in Cursor because Cursor already supports the two behaviors they rely on:

- persistent repo instructions via `.cursorrules`
- file-scoped prompting via `@filename`
- multi-file execution via Composer in Agent mode

Treat the workflow files as operating instructions for Cursor, not as a plugin you install.

### What To Reference

For implementation work, load the Developer workflow files:

```text
@Developer Workflow/.github/.workflow/README.md
@Developer Workflow/.github/.workflow/workflow.md
@Developer Workflow/.github/.workflow/hooks/IDE-EXECUTION-GUIDE.md
```

For validation work, load the QA workflow files:

```text
@QA Workflow/.qa-workflow/README.md
@QA Workflow/.qa-workflow/workflow.md
@QA Workflow/.qa-workflow/hooks/IDE-EXECUTION-GUIDE.md
```

### Recommended Cursor Setup

Create a `.cursorrules` file in the target project and keep it short:

```text
Use the attached workflow files as the operating model for this repository.

Rules:
- Prefer spec paths, diffs, changed files, and short logs over full file dumps
- When I request a hook, use the referenced hook prompt file exactly
- When I request a chain, guide me step by step and stop at approval gates
- Reuse workflow snippets, hooks, specs, and memory files before inventing new prompts
- Keep responses scoped to the current phase
```

This gives Cursor a stable baseline every session.

### Developer Workflow In Cursor

Use Composer in Agent mode for most workflow-driven tasks.

```text
@Developer Workflow/.github/.workflow/README.md
@Developer Workflow/.github/.workflow/hooks/IDE-EXECUTION-GUIDE.md

Use this workflow as my development operating model.
When I say "execute hook" or "execute chain", follow the referenced workflow files.
Prefer spec paths, changed files, diffs, and memory files over pasted code.
```

#### Example: Start A New Feature

```text
@Developer Workflow/.github/.workflow/hooks/chains/README.md

Run the "new-feature" chain for "payment-idempotency".
Take me step by step and stop at approval gates.
```

#### Example: Run A Single Hook

```text
@Developer Workflow/.github/.workflow/hooks/implementation/pre-implementation.scope-check.prompt.md

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
@Developer Workflow/.github/.workflow/hooks/quality/review-diff.prompt.md

Review this diff against the approved spec.
Spec: docs/specs/payment-idempotency.md
Diff: <paste git diff>
Tests: <short test summary>
Return severity-ranked findings only.
```

### QA Workflow In Cursor

Use the QA workflow after implementation, during test design, or near release.

```text
@QA Workflow/.qa-workflow/README.md
@QA Workflow/.qa-workflow/hooks/IDE-EXECUTION-GUIDE.md

Use this QA workflow as my validation operating model.
When I request a hook or chain, follow the referenced prompt files.
Keep outputs findings-first, diff-focused, and scoped to changed modules.
```

#### Example: QA Analysis After Spec

```text
@QA Workflow/.qa-workflow/hooks/spec/post-spec.qa-analysis.prompt.md
@QA Workflow/.qa-workflow/qa-context/test-strategy.md

Execute this hook.
Spec: docs/specs/payment-idempotency.md
Changed modules:
- src/payments/service.ts
- src/payments/controller.ts
```

#### Example: Generate Targeted QA Tests

```text
@QA Workflow/.qa-workflow/hooks/test/post-implementation.generate-tests.prompt.md
@QA Workflow/.qa-workflow/snippets/qa/api_contract_test.snippet.md

Generate targeted QA tests for the approved spec and changed modules only.
Spec: docs/specs/payment-idempotency.md
Diff summary: idempotency key added to create payment flow
```

#### Example: Release Validation

```text
@QA Workflow/.qa-workflow/hooks/release/release-readiness-review.prompt.md

Run release readiness review.
Spec: docs/specs/payment-idempotency.md
Test summary: <short summary>
Open risks: <short list>
```

### Fast Operating Pattern

Use this loop in Cursor for both workflows:

1. Reference the workflow file with `@...`
2. Reference the spec path instead of repeating the requirement
3. Pass only changed files, diff summary, and short logs
4. Run one hook or one chain at a time
5. Save durable outputs into spec, memory, or context files

### When To Use Inline Edit Vs Composer

```text
Use Ctrl+K / Cmd+K:
- rewrite a small function
- clean up a test
- make a change under ~20 lines

Use Composer in Agent mode:
- execute a workflow chain
- coordinate spec + code + tests
- update multiple files from a hook result
- run review or QA passes across a diff
```

### Cursor-Specific Best Practices

- Use `@filename` references instead of pasting full files
- Keep each workflow prompt phase-specific; do not combine requirement, implementation, review, and QA in one request
- Prefer one Composer session per task stream so approvals and gates stay clear
- Switch to a stronger model for spec creation, review, debugging, and QA analysis
- Use cheaper models for formatting, short rewrites, and lightweight edits
- If the chat gets noisy, open a fresh Composer session and reload only the relevant workflow files
