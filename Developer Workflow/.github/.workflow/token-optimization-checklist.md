# Token Optimization Checklist

Use this checklist before every AI interaction.

## Context Hygiene

- [ ] I am sending file paths, not full files.
- [ ] I am sending one function/snippet, not modules.
- [ ] I referenced spec path instead of restating requirements.
- [ ] I removed unrelated logs/content.

## Prompt Reuse

- [ ] I used a snippet from `snippets/`.
- [ ] I avoided rewriting agent instructions.
- [ ] I requested structured output only.

## Execution Scope

- [ ] Output asks for patch/diff only.
- [ ] Test generation is limited to changed behavior.
- [ ] Debug request includes only failing signal + local code.

## Gate Compliance

- [ ] Requirement gate approved.
- [ ] Impact gate approved.
- [ ] Spec gate approved.
- [ ] Pre-implementation gate approved.
- [ ] Pre-PR gate approved.

## Memory Discipline

- [ ] Reusable knowledge saved to `ai-memory/`.
- [ ] Chat avoids carrying long historical context.
