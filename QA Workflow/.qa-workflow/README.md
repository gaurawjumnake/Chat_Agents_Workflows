# QA-Assisted Validation System (.qa-workflow)

This folder is a migration of the developer workflow into a QA-first, validation-centric operating model for IDE chat agents.

## What Changed

- Kept: Spec-DD gates, memory model, hooks, snippets, token controls.
- Removed: implementation-only planning and code-generation-centric flow.
- Added: functional/API/security/regression/healing/release QA workflows.

## Start Here

1. Read `workflow.md` for QA phases and gates.
2. Use `hooks/README.md` and `hooks/CATALOG.md` for executable prompts.
3. Use `agents/*.agent.md` for role-specific QA tasks.
4. Use `snippets/qa/*.snippet.md` to run practical QA prompts.
5. Store long-term findings in `qa-memory/`.
6. Keep validation strategy artifacts in `qa-context/`.

## Folder Map

- `workflow.md`: End-to-end QA lifecycle with hard gates.
- `agents/`: QA role definitions.
- `snippets/qa/`: Reusable QA prompts.
- `hooks/`: Triggered QA prompt hooks.
- `qa-context/`: Test strategy, regression and security context.
- `qa-memory/`: Persistent QA knowledge.
- `specs/`: Shared spec compatibility and templates.
- `examples/`: End-to-end QA run example.
- `migration-summary.md`: Keep/remove/convert/create decisions.

## Compatibility

This QA workflow remains compatible with:
- specs
- context memory
- hooks
- snippets
- IDE chat execution patterns

## Operating Principle

Spec first. Diff-focused validation. Incremental context updates. Approval gates before release.
