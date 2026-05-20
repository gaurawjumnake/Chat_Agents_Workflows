# Token-Optimized AI Workflow (Generalized, Spec-DD First)

## 1) Workflow Phases

| Phase | Inputs | Outputs | Allowed Context Size (Strict) | AI Usage |
|---|---|---|---|---|
| 1. Requirement Understanding | Requirement note, constraints, acceptance criteria | Requirement summary + open questions | <= 500 tokens | Use AI for summarization and ambiguity detection |
| 2. Impact Analysis | Entry points, changed API/feature, repo map | Impacted files/modules only | <= 700 tokens | Use AI for dependency tracing; avoid full file dumps |
| 3. Spec Creation | Confirmed requirement + impact list | Feature spec in `specs/<feature>.md` | <= 900 tokens | Use AI for structured spec drafting |
| 4. Design | Approved spec + interfaces | Design note: architecture, contracts, migration plan | <= 900 tokens | Use AI for option comparison; avoid coding |
| 5. Implementation | Approved spec + target files + diff baseline | Minimal patch aligned to spec | <= 1200 tokens | Use AI for scoped code generation |
| 6. Testing | Spec test scenarios + changed functions | Unit/integration tests + expected results | <= 900 tokens | Use AI for test generation and edge cases |
| 7. Debugging | Failing test/logs + narrow code snippet | Root cause + fix patch + regression test | <= 900 tokens | Use AI for hypothesis and patch proposals |
| 8. Refactoring | Existing code + constraints + tests | Safer structure, no behavior change, updated tests | <= 1000 tokens | Use AI with strict non-functional boundaries |
| 9. Code Review | Diff + spec + tests | Findings list (bugs/risks), required fixes | <= 900 tokens | Use AI for risk-focused review |
| 10. Documentation | Approved changes + spec | Updated docs/changelog/runbook | <= 700 tokens | Use AI for concise, user-focused docs |

### Phase AI-Avoid Conditions

- Avoid AI when legal/compliance interpretation requires official policy wording.
- Avoid AI for large binary artifacts or generated files.
- Avoid AI when context exceeds budget and cannot be reduced via spec + diff + snippets.

## 2) Spec-Driven Development Rules

- No coding without spec approval.
- Spec is single source of truth during implementation.
- Never re-send raw requirement after spec approval; reference spec path only.
- Keep specs modular and file-scoped when possible.

Spec location: `specs/<feature-name>.md`

## 3) Gated Workflow (Hard Stops)

1. After Requirement: stop for user confirmation.
2. After Impact: stop for user confirmation.
3. After Spec: stop for mandatory approval.
4. Before Implementation: stop for confirmation.
5. Before PR: stop for confirmation.

If gate is not approved, do not proceed.

## 4) IDE Execution Model

- Snippets = prompt templates for repeatable tasks.
- Specs = source of truth for implementation.
- Obsidian (`ai-memory/`) = long-term memory.
- Chat = short-term reasoning only.

## 5) Token Control Strategy

- Operate on function snippets, file paths, and diffs.
- Never send full files unless unavoidable.
- Prefer `impacted files list` over code body.
- Reuse snippet templates exactly.
- Link to memory/spec files instead of retyping history.

## 6) Task-Specific Instruction Matrix

| Task Type | Required Inputs | Required Snippet | Output Constraint |
|---|---|---|---|
| New feature | Requirement + impact list + approved spec | `create_spec` then `implement_from_spec` | Only changed code and tests |
| Bug fix | Logs + failing test + narrowed snippet | `debug_issue` | Root cause + minimal fix |
| Refactor | Current function + behavior constraints + tests | `impact_analysis` + implementation snippet | No behavior change |
| API change | Contract delta + affected endpoints | `impact_analysis` + `create_spec` | Contract, migration, tests |
| Code review | Diff + spec reference | `review_diff` | Findings first, no rewrite |

## 7) Deliverables Mapping

- Full workflow design: this file.
- Agent definitions: `agents/*.agent.md`.
- Snippet library: `snippets/*.snippet.md`.
- Obsidian structure: `ai-memory/README.md` + folders.
- Hook definitions: `hooks/hook-model.md`.
- End-to-end example: `examples/end-to-end-flow.md`.
- Practical daily usage: `daily-usage-guide.md`.
- Token checklist: `token-optimization-checklist.md`.
