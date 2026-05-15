# AI-Assisted Development System (.copilot)

This folder contains a complete, practical, token-optimized workflow for IDE chat agents (Copilot Chat, Claude, Codex) that is tech-stack agnostic and task-oriented.

## Copilot Summary

Use this folder as your single entry point for running Spec-DD inside GitHub Copilot Chat.

- `README.md` is the operator guide and first touchpoint.
- `workflow.md` defines the 10 development phases and strict gates.
- `hooks/` provides reusable auto-trigger prompt hooks for each workflow stage.
- `agents/` provides lightweight role prompts for focused tasks.
- `snippets/` provides copy-paste prompt templates to avoid ad-hoc prompting.
- `specs/` is the source of truth for implementation scope.
- `ai-memory/` stores long-term context so chat stays short and token-efficient.

Core operating model in Copilot:
1. Start with context and spec references, not full code dumps.
2. Run hook + agent + snippet in small scoped steps.
3. Save decisions/patterns/bugs after each phase.
4. Stop at gates for approval before implementation and PR.

## Start Here

1. Read `workflow.md`.
2. Use `hooks/hook-model.md` for trigger-driven execution.
3. Use `agents/*.agent.md` for role-specific invocation.
4. Use `snippets/*.snippet.md` as copy-paste prompt templates.
5. Store long-term context in `ai-memory/` (Obsidian-compatible).
6. Keep feature specs in `specs/` and gate implementation on approved specs.

## Folder Map

- `workflow.md`: Full workflow design and execution model.
- `agents/`: Lightweight agent instruction blocks.
- `snippets/`: Reusable prompt snippets.
- `specs/`: Spec-DD template and approved specs.
- `hooks/`: Hook trigger definitions.
- `ai-memory/`: Persistent memory layer for Obsidian.
- `examples/`: End-to-end practical flow.
- `daily-usage-guide.md`: Day-to-day operation.
- `token-optimization-checklist.md`: Token safety controls.

## Task-Wise Usage Examples (Copilot Chat)

Use these as direct prompts in Copilot Chat.

### 1) New Feature Implementation

Goal: Build a new feature with full Spec-DD discipline.

Prompt:
```text
Execute chain: hooks/chains/new-feature
Feature: <feature-name>
Spec path: .copilot/specs/<feature-name>.md
```

Expected flow:
- pre-task context load
- impact memory snapshot
- post-requirement auto-create spec
- spec approval gate
- pre-implementation scope check
- implementation context update
- test generation
- review and pre-PR gate

### 2) Bug Fix

Goal: Diagnose quickly, patch minimally, and prevent recurrence.

Prompt:
```text
Execute chain: hooks/chains/bug-fix
Feature/Bug: <bug-key>
Logs: <trimmed logs>
```

Expected flow:
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
