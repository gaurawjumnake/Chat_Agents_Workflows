# QA Token Optimization Checklist

## Context Hygiene

- [ ] I referenced spec path instead of restating requirements.
- [ ] I used impacted modules and diffs, not full files.
- [ ] I included only failed traces relevant to the failing test.
- [ ] I removed unrelated logs and historical chat text.

## Prompt Reuse

- [ ] I used an existing hook/snippet from this workflow.
- [ ] I requested structured output only.
- [ ] I kept inputs aligned to one QA objective.

## Scope Control

- [ ] Regression request is scoped to changed modules.
- [ ] Security request is scoped to exposed attack surface.
- [ ] Healing request includes root cause evidence and guardrails.

## Gate Compliance

- [ ] Risk analysis gate approved.
- [ ] Test strategy gate approved.
- [ ] Security validation gate approved.
- [ ] Healing recommendation gate approved (if used).
- [ ] Release validation gate approved.

## Memory Discipline

- [ ] Bug/security/healing findings saved to `qa-memory/`.
- [ ] Regression scope updates saved to `qa-context/regression-map.md`.
- [ ] I reused prior memory notes before new analysis.
