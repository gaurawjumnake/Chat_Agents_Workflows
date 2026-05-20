# Coding Standards & Development Rules

## Universal Coding Standards

All code changes MUST adhere to:

### 1. **Spec Alignment**
- Every change must reference an approved spec
- Spec path must be cited in commit message, PR description, code comments
- No "quick fixes" without a spec (create minimal spec if needed)

### 2. **Minimal Changes**
- Change only what's necessary to meet spec
- Do not refactor adjacent code unless spec authorizes it
- Minimize diff size (easier review, fewer bugs)

### 3. **Backward Compatibility**
- Breaking changes must be:
  - Documented in spec as "Breaking Changes" section
  - Listed in changelog
  - Approved via `quality.detect-breaking-changes` hook
  - Paired with migration path or deprecation notice

### 4. **Testing Coverage**
- Every feature change requires tests
- Tests MUST cover:
  - Happy path (spec scenarios)
  - Edge cases (spec edge cases section)
  - Error handling
  - Backward compatibility (if applicable)
- Test coverage target: 80%+ for changed code

### 5. **Documentation Requirements**
- Update docstrings for modified functions
- Update README if API changes
- Update changelog (format: see `post-pr.changelog-update` hook)
- Add architecture notes if design is non-obvious

### 6. **Code Quality**
- Linter must pass (no warnings)
- No duplicated code (extract to shared function if 3+ occurrences)
- No hardcoded values (use constants)
- Comments for "why" not "what" (code shows what; comments explain why)

### 7. **Performance Considerations**
- Query count must not increase (N+1 prevention)
- Memory usage must not increase (no unbounded loops/arrays)
- Load time must not degrade >5% (benchmark if needed)
- Report performance impact in PR description

### 8. **Security Checklist**
- No hardcoded secrets or credentials
- SQL queries must be parameterized
- User input must be validated/sanitized
- Error messages must not leak sensitive info
- Approval via `security-review` hook if handling PII/secrets

## Language-Specific Standards

### Python
```
- Type hints required (mypy strict mode)
- Docstrings for all public functions (Google style)
- f-strings for string formatting
- pathlib for file operations (not os.path)
- Use context managers for resource cleanup
```

### TypeScript/JavaScript
```
- Strict mode required
- Type hints required (no `any`)
- Async/await (not .then())
- Error handling required (no silent failures)
- ESLint must pass
```

### SQL
```
- Parameterized queries always
- Migrations for schema changes
- Index performance analysis required
- Query complexity limits (no N+1, no full table scans without LIMIT)
```

### Go
```
- Error returns checked (no ignored errors)
- Goroutine leaks prevented (proper cleanup)
- Concurrency patterns documented
- Tests for concurrent behavior
```

## File Organization

### Structure
```
src/
  <domain>/
    <module>/
      __init__.py        (public interface only)
      core.py           (main logic)
      models.py         (data structures)
      exceptions.py     (domain exceptions)
      tests/
        test_core.py
        test_models.py
        fixtures/       (test data)
tests/
  integration/          (cross-module tests)
  fixtures/             (shared test data)
docs/
  <domain>/
    README.md
    architecture.md     (design decisions)
    api.md             (public interface)
```

### Naming Conventions
- Files: `snake_case.py`, `kebab-case.ts`
- Classes: `PascalCase`
- Functions: `snake_case()`
- Constants: `UPPER_CASE`
- Private: `_leading_underscore`

## Git Commit Standards

### Commit Message Format
```
<type>: <short description (50 chars max)>

<detailed explanation (if needed, wrap at 72 chars)>

Spec: <path/to/spec.md> (or "No spec")
Fixes: <issue ID if applicable>
Relates-to: <other related issue/spec>
```

### Commit Types
- `feat`: New feature (requires approved spec)
- `fix`: Bug fix (reference bug spec or issue)
- `refactor`: Code restructuring (no behavior change)
- `test`: Test additions/fixes
- `docs`: Documentation updates
- `chore`: Build/tooling changes

### Commit Size Limits
- Keep commits small and focused (1 concern per commit)
- Max 400 lines changed per commit (simpler review)
- If split needed: organize by layer (models → logic → tests)

## Code Review Standards

### Reviewer Responsibilities
- Verify spec alignment (is spec path cited?)
- Check test coverage (are edge cases tested?)
- Scan for breaking changes (run `quality.detect-breaking-changes`)
- Verify backward compatibility
- Check security concerns (any PII/credentials exposed?)

### Approval Criteria
- ✓ Spec-aligned
- ✓ Tests passing (80%+ coverage)
- ✓ No new warnings/linter issues
- ✓ Documentation updated
- ✓ Performance impact acceptable
- ✓ Security review approved (if needed)

### Blocking Issues
- ✗ Missing spec or broken spec reference
- ✗ Test coverage below 75%
- ✗ Linter warnings present
- ✗ Breaking change without migration path
- ✗ Performance degradation >5%
- ✗ Security issues unfixed

## Documentation Standards

### README.md
- Brief project description (1-2 sentences)
- Quick start (3-5 steps)
- API overview (what can developers do?)
- Configuration guide
- Troubleshooting (common issues)

### Docstrings (Code Comments)
```python
def process_payment(order_id: str, amount: float) -> Receipt:
    """
    Process a payment for the given order.
    
    Idempotent: calling twice with same order_id returns same receipt.
    
    Args:
        order_id: Unique order identifier
        amount: Payment amount in cents
        
    Returns:
        Receipt with transaction ID and status
        
    Raises:
        InsufficientFundsError: If account balance < amount
        PaymentDeclinedError: If processor rejects transaction
        
    Spec: specs/payment-processing.md
    """
```

### Architecture Notes
- Location: `docs/<domain>/architecture.md`
- Content:
  - Design decisions (why this approach?)
  - Trade-offs (what alternatives were considered?)
  - Integration points (how does this connect to other modules?)
  - Future extensions (what might change?)

## Version Control

### Branching Strategy
- `main`: Production code (always deployable)
- `develop`: Integration branch
- `feature/<spec-name>`: Feature branches
- `bugfix/<bug-id>`: Bug fix branches
- `hotfix/<issue-id>`: Emergency fixes

### Branch Naming
- `feature/oauth2-integration`
- `bugfix/payment-token-expiry`
- `hotfix/database-connection-leak`

### PR Standards
- Title: `[<type>] <spec-name>: <short description>`
- Description: Must include:
  - Link to spec
  - Summary of changes
  - Testing notes
  - Deployment notes (if any)
- Must pass all status checks before merge
- Minimum 1 approval (2 for security/schema changes)

## Approval Gates

This workflow enforces **hard stops** at critical points:

1. **Spec Approval** (after spec draft)
   - Required before any implementation
   - Requires explicit approval (cannot proceed without)

2. **Pre-Implementation Scope Check** (before coding)
   - Validates that affected files match spec scope
   - Prevents scope creep

3. **Pre-PR Readiness Gate** (before PR)
   - Tests passing
   - Documentation complete
   - Coverage acceptable
   - Security approved (if needed)
   - Cannot be bypassed

## Violations & Escalation

### Minor Violations
- Linter warnings → Fix in next commit
- Missing docstring → Add before merge
- Coverage <80% → Increase coverage

### Major Violations
- Missing spec → Create spec (gate blocks merge)
- Breaking change undocumented → Update spec (gate blocks merge)
- Security issue → Fix + security review (gate blocks merge)
- Test failure → Debug + fix (gate blocks merge)

### Escalation Path
1. Reviewer flags issue
2. Author fixes or provides justification
3. If disagreement: escalate to team lead
4. Team lead makes final decision (documented in PR)
