# Daily QA Usage Guide

**See [workflow-orchestrator.md](workflow-orchestrator.md) for full details.**

## 1-Minute Quick Start

1. Open [workflow-orchestrator.md](workflow-orchestrator.md)
2. Find your phase (1-10)
3. Load inputs from phase section
4. Run agent/hook/snippet
5. Check gate criteria
6. Archive to `qa-memory/`

## Fast Execution Loop (New Feature)

```
Phase 1: Requirement QA → agents/requirement-qa.agent.md
Phase 2: QA Impact → agents/qa-impact.agent.md
Phase 3-4: Spec QA + Risk → hooks/spec/*.prompt.md
Phase 5: Test Design → hooks/test/*.prompt.md
Phase 7: Regression → hooks/regression/*.prompt.md
Phase 8: Security → hooks/security/*.prompt.md
Phase 10: Release → hooks/release/*.prompt.md
```

## Minimal Inputs Per Phase

- **Phases 1-2**: Requirement summary + entry points
- **Phase 3-4**: Spec path + qa-context/ references
- **Phase 5-7**: Diff + changed modules + existing tests
- **Phase 8**: Security model + threat findings
- **Phase 10**: Smoke test results + risk register

## Archive Points

- After every major gate approval → `qa-memory/approvals/`
- Found a bug → `qa-memory/bugs/`
- Found security issue → `qa-memory/security/`
- Analyzed flaky test → `qa-memory/healing/`
- Updated regression scope → `qa-memory/regression/`

## Token Optimization

- ✅ Reference specs by path (save 400+ tokens)
- ✅ Use diffs, not full files (save 600+ tokens)
- ✅ Load only needed qa-context (save 200+ tokens)
- ✅ Reuse qa-memory findings (save 300+ tokens)

---

**Full details:** [workflow-orchestrator.md](workflow-orchestrator.md)
