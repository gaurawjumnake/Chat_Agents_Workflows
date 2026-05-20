# Daily Usage Guide (Quick Reference Only)

⚠️ **NOT AN ENTRY POINT** - See [workflow-orchestrator.md](workflow-orchestrator.md) for authoritative workflow definitions.

This is a **practical checklist** for the standard daily workflow loop. Use alongside the orchestrator.

---

## Fast Daily Loop

1. Start task with Requirement Agent and save a short requirement summary.
2. Run Impact Agent to produce impacted files/modules only.
3. Draft spec with Spec Agent and pause for approval.
4. After approval, run Design Agent for contracts and approach.
5. Implement with Implementation Agent using spec path only.
6. Generate tests with Test Agent from spec scenarios.
7. If failure occurs, run Debug Agent with logs + narrow snippet.
8. Run Review Agent on diff before PR.
9. Finish with Documentation Agent for changelog/docs.

## What To Paste Into Chat (Minimal)

- Task objective (1-2 lines)
- Spec path
- Target file paths
- Current diff summary or failing test/log

## What Not To Paste

- Entire files
- Full chat history
- Repeated requirements after spec approval

## Daily Standup Maintenance (5 minutes)

- Add 1 decision note in `ai-memory/decisions/`.
- Add 1 pattern or bug entry if discovered.
- Archive stale notes monthly.

## Team Adoption

- Keep snippet names standard.
- Enforce gates in code review process.
- Reject implementation PRs without approved spec reference.
