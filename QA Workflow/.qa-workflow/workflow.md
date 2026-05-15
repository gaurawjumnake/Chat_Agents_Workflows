# Token-Optimized QA Workflow (Spec-DD Compatible)

## 1) QA Workflow Phases

| Phase | Inputs | Outputs | Max Context | QA Focus |
|---|---|---|---|---|
| 1. Requirement QA | Requirement note, acceptance criteria | Testable requirement summary + ambiguities | <= 500 | Validatability and testability |
| 2. QA Impact Analysis | Entry points, APIs, modules | Affected test scope + risk map | <= 700 | Blast radius and test scope |
| 3. Spec QA Analysis | Approved/working spec | QA strategy overlay + test matrix gaps | <= 900 | Functional/API/security coverage |
| 4. Risk & Threat Analysis | Spec + impact map | Risk register + threat model updates | <= 900 | Abuse cases and release risk |
| 5. Test Design | Spec scenarios + diff | Targeted functional/API/automation tests | <= 1000 | Coverage over implementation details |
| 6. Test Execution & Debug | Failures, logs, artifacts | Root cause + bug evidence + regression additions | <= 900 | Fast triage and reproducibility |
| 7. Regression Intelligence | Spec delta + changed modules | Targeted regression subset | <= 800 | Avoid full-suite reruns |
| 8. Security Validation | Auth/RBAC/contracts/deps/secrets | Security verdict + required actions | <= 900 | Pre-release security checks |
| 9. Healing Analysis | Flaky/failing test telemetry | Healing proposal + approval gate | <= 800 | Safe self-healing recommendations |
| 10. Release Validation | Smoke pack + unresolved risks | Go/No-Go decision | <= 700 | Release readiness |

## 2) Core Rules

- No release validation without spec and QA analysis.
- Reference specs by path, avoid repeating raw requirements.
- Use diffs and changed modules, not full repo scans.
- Archive every major QA decision in `qa-memory/`.
- Security and healing outputs require explicit approval before action.

## 3) Hard Approval Gates

1. After requirement QA summary.
2. After spec QA analysis.
3. After risk/threat analysis.
4. Before security validation sign-off.
5. Before applying healing patches.
6. Before release go/no-go.

## 4) IDE Execution Model

- Snippets = repeatable QA prompts.
- Hooks = phase transitions and artifact persistence.
- QA context = stable strategy layer.
- QA memory = long-term bug/risk/pattern intelligence.

## 5) Token Controls

- Specs before code.
- Diffs before full files.
- Incremental updates only.
- No repo-wide rescans unless impact cannot be derived.
- Reuse hooks and snippets to keep prompts modular.

## 6) Deliverables

- Agents: `agents/*.agent.md`
- Snippets: `snippets/qa/*.snippet.md`
- Hooks: `hooks/**/*.prompt.md`
- QA context: `qa-context/*.md`
- QA memory templates: `qa-memory/**`
