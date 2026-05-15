# QA Hook Integration Guide

## How Hooks Integrate with QA Workflow

- `workflow.md` defines phases and gates.
- `hooks/*.prompt.md` execute each phase with token bounds.
- `agents/*.agent.md` provide role-specific depth when a hook requires deeper analysis.
- `qa-context/` stores stable strategy artifacts.
- `qa-memory/` stores historical findings to avoid re-analysis.

## Team Adoption Checklist

- Use spec path references in every validation step.
- Enforce risk, security, healing, and release gates.
- Require findings-first outputs for review hooks.
- Archive findings after each major validation stage.

## Troubleshooting

- If prompts are too long, split by hook and enforce token budgets.
- If scope is unclear, rerun `pre-task/context-load` and `post-impact/memory-snapshot`.
- If release gate fails, resolve blockers and rerun only the impacted hooks.
