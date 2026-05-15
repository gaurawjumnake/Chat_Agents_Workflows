# Chat Agents Workflows

This repository contains practical workflow templates for AI-assisted software delivery, with two tracks:

- Developer Workflow for implementation-focused Spec-Driven Development
- QA Workflow for validation-focused quality gates and release readiness

The operating model is token-optimized, context-first, and approval-gated. It is designed to reduce repeated repo scanning, improve answer quality, and keep long-running work maintainable.

## Repository Summary

The repository combines:

- Root strategy guides for model selection and token efficiency
- Prompt templates for context building and graph-based code understanding
- A full Developer workflow with phases, hooks, agents, snippets, and memory
- A full QA workflow with phases, hooks, agents, snippets, context, and memory

Primary intent:

- Use specs as the source of truth
- Use context files and memory instead of repeating raw code
- Use hooks and snippets to standardize execution
- Use approval gates before high-risk steps

## Key Files At Root

- 1. Chat Agent Usage Guide.txt: token efficiency principles, graph-first usage, and practical prompting discipline
- 2. MODEL SELECTION GUIDE.txt: model choice matrix by task type
- 4. prompt to build context.md: create or update ai-context knowledge files
- 5. prompt to merge context.md: integrate ai-context into all workflow phases
- 6.1 prompt to incorporate code-flow.txt: apply Codeflow graph snapshot rules
- 6.2 prompt to incorporate code-review-graph.txt: apply live code-review-graph MCP rules
- 6.3 prompt to incorporate graphify.txt: apply Graphify MCP graph-query rules
- 7. prompt to update QA workflow for ui-ux testing.md: extend QA workflow with UI/UX testing phases, visual regression, accessibility, responsive and healing layers
- 8. prompt to update developer workflow for UI developer.md: extend Developer workflow with Figma-driven, component-based, and accessibility-aware frontend implementation phases

## Recommended Prompt Sequence

Use prompts in this exact order, and only apply advanced prompts when their required graph system is available.

1. Run 4. prompt to build context.md
2. Run 5. prompt to merge context.md
3. Run 6.1 prompt to incorporate code-flow.txt (if Codeflow graph snapshot exists)
4. Run 6.2 prompt to incorporate code-review-graph.txt (if code-review-graph MCP is available)
5. Run 6.3 prompt to incorporate graphify.txt (if Graphify MCP tools are available)
6. Run 7. prompt to update QA workflow for ui-ux testing.md (if your project includes UI/UX QA)
7. Run 8. prompt to update developer workflow for UI developer.md (if your project includes frontend/UI development)

Why this order:

- Prompt 4 builds the reusable context layer
- Prompt 5 makes workflows consume that layer
- Prompts 6.1 to 6.3 enforce graph-first discovery and safer change impact analysis
- Prompt 7 extends QA with UI/UX validation, visual regression, accessibility and healing layers
- Prompt 8 extends the Developer workflow with Figma-driven, component-based and responsive implementation phases

## Developer Workflow Overview

Location: Developer Workflow/.github/.workflow/

Core phase flow:

1. Requirement Understanding
2. Impact Analysis
3. Spec Creation (mandatory approval)
4. Design
5. Implementation
6. Testing
7. Debugging
8. Refactoring
9. Code Review
10. Documentation

Execution principle:

- Hook + agent + snippet in small scoped steps
- Diff-focused context
- No implementation before approved spec

### UI/Frontend Extension (prompt 8)

Run prompt 8 to add a frontend development layer on top of the base workflow.

Additional phases:

1. UI/UX Requirement Analysis
2. Figma and Image Analysis
3. Component Impact Analysis
4. Frontend Architecture Planning
5. Design System Mapping
6. Responsive Planning
7. Component Implementation
8. State and Interaction Implementation
9. Accessibility Validation
10. Frontend Testing
11. Visual Review
12. Storybook and Documentation Update
13. PR and UI Review

New assets added by prompt 8: Figma analysis agents, component-focused snippets, design consistency hooks, UI context files, responsive and accessibility workflows.

## QA Workflow Overview

Location: QA Workflow/.qa-workflow/

Core phase flow:

1. Requirement QA
2. QA Impact Analysis
3. Spec QA Analysis
4. Risk and Threat Modeling
5. Test Design
6. Test Execution and Debug
7. Regression Intelligence
8. Security Validation
9. Healing Analysis
10. Release Validation

Execution principle:

- Validation-first with hard release gates
- Targeted regression over broad reruns
- Persistent QA memory for reuse across cycles

### UI/UX QA Extension (prompt 7)

Run prompt 7 to add a dedicated UI/UX validation layer on top of the base QA workflow.

Additional phases:

1. UI Impact Analysis
2. UX Flow Validation
3. Accessibility Review
4. Responsive Validation
5. Interaction Testing
6. Design System Validation
7. Visual Regression Analysis
8. Frontend Performance UX Validation
9. UI Healing
10. Release UX Validation

New assets added by prompt 7: visual QA agents, accessibility and responsive QA agents, UI/UX hooks, UI/UX snippets, visual regression memory, frontend healing workflow with mandatory approval gates.

## Important Instructions

These are mandatory operating rules for this repository.

1. Use prompts in sequence and by requirement.
2. Do not skip prompt 4 or prompt 5 for new repository onboarding.
3. Do not use prompt 6.1, 6.2, or 6.3 unless the corresponding graph capability is available.
4. No implementation without approved spec.
5. Use context files, memory files, and diffs before opening large code areas.
6. Reuse existing hooks, agents, and snippets before writing ad-hoc prompts.
7. Stop at required approval gates before implementation, security actions, healing actions, and release decisions.
8. Keep updates incremental: refine context and memory files instead of rewriting everything.
9. Prefer targeted scans and impacted-file analysis; avoid full-repository rescans during normal task flow.
10. Keep this repository current as workflows evolve.

## Ongoing Update Policy

This repository will continue to receive new changes.

When updating:

- Preserve the phase order and gate model
- Add new prompts/hooks/agents/snippets with clear purpose and placement
- Update workflow summaries when behavior changes
- Record durable patterns and decisions in memory folders

## Quick Start

1. Read the Developer or QA workflow README based on your task type.
2. Execute prompt 4 then prompt 5.
3. Enable graph prompts 6.1 to 6.3 only if corresponding graph tooling is available.
4. Start work from requirement and impact phases, then proceed through gates.
