# QA Hook Token Optimization Strategy

## Principles

1. Specs before code dumps.
2. Diffs and changed modules before full-file context.
3. Memory snapshots prevent re-scanning.
4. Regression targeting avoids full-suite defaults.
5. Security/healing prompts stay scoped to changed surface.

## Savings Patterns

- Reuse `qa-context/` and `qa-memory/` references instead of re-describing state.
- Use hook outputs as direct input to the next hook.
- Keep structured outputs with fixed sections.

## Anti-Patterns

- Repo-wide rescans for localized changes.
- Passing full CI logs when failure slices are enough.
- Repeating requirement prose after spec link is available.
