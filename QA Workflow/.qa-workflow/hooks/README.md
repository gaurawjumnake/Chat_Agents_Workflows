# QA Hook System: Master Index

Hooks are copy-paste prompt templates that preserve token-efficient, gate-driven QA execution.

## Quick Start

1. Review `CATALOG.md`.
2. Execute a chain from `chains/README.md`.
3. Follow gate stops (risk, security, healing, release).

## Core QA Hooks

- Pre-task: `pre-task/context-load`
- Post-impact: `post-impact/memory-snapshot`
- Spec QA: `spec/post-spec.qa-analysis`, `spec/post-spec.risk-analysis`
- Testing: `test/post-implementation.generate-tests`
- Regression: `regression/regression-scope-update`
- Security: `security/pre-release.security-validation`
- Healing: `healing/post-failure.healing-analysis`
- Release: `release/pre-release.smoke-validation`, `release/release-readiness-review`
- Review: `review/pre-review.spec-check`, `review/post-review.findings-archive`

## Shared Principles

- Diff/spec/context first
- No repo-wide rescans
- Memory-backed incremental updates
- Approval gates at critical risk points
