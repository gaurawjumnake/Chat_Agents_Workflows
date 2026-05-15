# Hook: post-pr.changelog-update

## Purpose
Automatically update CHANGELOG.md based on spec.

## Trigger
PR merged to main.

## Input
- Feature name
- Spec path
- PR URL

## Prompt Template (Copy-Paste)

```
HOOK: Post-PR Changelog Update

Feature: <feature-name>
Spec: specs/<feature-name>.md
PR: <url>
Date: 2026-05-15

From spec, extract user-facing changes.

Update CHANGELOG.md section:

## [Unreleased]

### Added
- <user-facing feature description from spec summary>

### Changed
- <API changes from spec contracts>

### Fixed
- <bugs fixed, if any>

Rules:
- Keep entries short and user-focused
- Link to spec in PR URL comment
- Group by feature or category

After merge, run this hook to keep changelog current.
```

## Expected Output
- Updated CHANGELOG.md section

## Token Rules
- User-facing language only
- Reference spec, not implementation
- Output <= 150 tokens
