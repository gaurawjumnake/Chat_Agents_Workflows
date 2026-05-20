# Memory System Usage & Incremental Updates

## Memory Structure

```
.workflow/memory/
  architecture/           # System design decisions
  workflows/              # Workflow optimization notes
  incidents/              # Bug analysis and root causes
  ui/                     # Design patterns and decisions
  security/               # Security decisions and audits
  patterns/               # Code patterns and idioms
  regression/             # Test failure analysis
  decisions.md            # Centralized decision log
```

## File Naming Convention

### Date-Prefixed Notes
Used for time-series data (bugs, decisions, incidents):
```
<YYYY-MM-DD>-<key>.md

Example:
  2026-05-20-oauth2-token-validation.md
  2026-05-19-payment-retry-loop-deadlock.md
```

### Static Notes
Used for patterns and long-lived information:
```
<name>.md

Example:
  async-context-manager-pattern.md
  sql-query-optimization-checklist.md
```

## Memory Templates

### Bug Analysis Template
**File**: `incidents/DATE-<bug-id>.md`

```markdown
# Bug: [Title]

## Date Found
[Date discovered]

## Symptom
[What went wrong? How did user notice?]

## Root Cause
[Why did it happen?]

## Impact
- Affected systems: [list]
- User impact: [describe]
- Severity: [Critical/High/Medium/Low]

## Fix Applied
[What code changed?]
- File: src/payment/validation.py
- Lines: 42-58
- Change: [brief description]

## Prevention
[What should we do differently next time?]

## Related Specs
- [Link to related spec if applicable]

## Lessons Learned
[Key takeaway for future reference]

## Testing
- Added test: test_payment_validation_edge_case.py:L42
- Test coverage: 100% for this fix

---
## Follow-up
- [ ] Post-mortem scheduled?
- [ ] Monitoring alert created?
- [ ] Similar patterns checked in codebase?
```

### Architectural Decision Record (ADR)
**File**: `decisions/DATE-<decision-key>.md`

```markdown
# ADR: [Decision Title]

## Date
[Date made]

## Context
[What situation led to this decision?]

## Decision
[What was decided?]

## Rationale
[Why this choice over alternatives?]

## Alternatives Considered
- [Alternative 1]: [Why rejected]
- [Alternative 2]: [Why rejected]

## Consequences
- **Positive**: [Benefits]
- **Negative**: [Trade-offs]

## Reversal Cost
[If we change our mind later, what's the effort?]

## Implementation
- Implemented in spec: [link]
- Related files: [list files affected]

## Related ADRs
- [Link to related decisions]

---
## Status
- [x] Approved
- [ ] Implemented
- [ ] Reversed
```

### Code Pattern Template
**File**: `patterns/<pattern-name>.md`

```markdown
# Pattern: [Pattern Name]

## Use Case
[When should we use this pattern?]

## Don't Use For
[When NOT to use this pattern?]

## Example
[Code snippet showing pattern]

```python
# Good example
@async_context_manager
class DatabaseConnection:
    async def __aenter__(self):
        await self.pool.open()
        return self
    
    async def __aexit__(self, *args):
        await self.pool.close()
```

## Anti-Pattern
[Code snippet showing what NOT to do]

```python
# Bad example - connection leak
db = Database()
connection = db.open()  # No cleanup!
```

## Performance Notes
[Does this pattern have performance implications?]

## Related Patterns
[Links to related patterns]

## When Introduced
[Which spec/PR introduced this pattern?]
```

### Test Failure Analysis Template
**File**: `regression/<test-name>-analysis.md`

```markdown
# Test Failure: [Test Name]

## Date
[When did this fail?]

## Test Location
- File: [path/to/test.py]
- Function: [test_function_name]

## Failure Message
[Full error output]

## Root Cause
[Why did this test fail?]

## Fix Applied
- [Describe fix]
- [Which file changed]

## Prevention
[How can we prevent this class of failure?]

## Related Tests
[Other tests that might fail for same reason?]

---
## Status
- [x] Fixed
- [x] Regression test added
- [ ] Monitoring alert set up
```

### Workflow Optimization Note
**File**: `workflows/<workflow-name>-optimization.md`

```markdown
# Workflow: [Workflow Name]

## Optimization Date
[Date optimized]

## Previous State
- Duration: X minutes
- Token usage: Y tokens
- Steps: [list]

## Optimization Applied
[What changed?]

## New State
- Duration: X minutes (was Y, saved Z%)
- Token usage: Y tokens (was Z, saved W%)
- Steps: [list]

## Technique
[Which optimization technique was used?]

## Results
- Before: [metrics]
- After: [metrics]
- Improvement: [% saved]

## Risks
[Any downsides to this optimization?]

## Reversible?
[Can we undo this if needed? How?]
```

## Incremental Update Rules

### Rule 1: Never Rewrite Existing Memory
**Old Way** (❌ Don't do this):
```
# Previous: [Old content]
# Now: [New content - replaces everything]
```

**New Way** (✓ Do this):
```
# Previous: [Old content - keep this]

## Update (2026-05-20)
[New findings or changes]
- What changed: [specific update]
- Why: [reason for change]
- Related PR: [link if applicable]
```

### Rule 2: Append New Findings
When a bug reoccurs or pattern extends:
```
# Bug: [Title]
## Date Found: 2026-03-15
[... original analysis ...]

---
## Recurrence (2026-05-20)
Found same issue in different module: src/payment/scheduler.py
[... new analysis ...]
```

### Rule 3: Link Related Notes
Instead of duplicating info:
```
# Bug: Payment token expiry

See also:
- Decision: [link to oauth2-token-handling.md]
- Pattern: [link to async-retry-pattern.md]
- Test: [link to test-payment-retry.md]
```

### Rule 4: Version Memory as Needed
When a pattern or decision becomes obsolete:
```markdown
# Decision: OAuth1 for API Auth [DEPRECATED]

⚠️ **Status**: DEPRECATED as of 2026-05-01

See new decision: [link to oauth2-for-api-auth.md]

Original content preserved below:
[... original ADR ...]
```

## Memory Loading Strategy

### Pre-Task Context Load
Before starting any task, load:
1. **Relevant specs** (feature being worked on)
2. **Related bugs** (past issues in same module)
3. **Related patterns** (code idioms for this domain)
4. **Related decisions** (architectural context)

Example:
```
Task: Implement session timeout feature

Load from memory:
✓ specs/session-management.md
✓ incidents/2026-04-15-session-token-leak.md
✓ patterns/async-context-manager-pattern.md
✓ decisions/2026-02-10-token-storage-decision.md
```

### Memory Loading Limits
- Max 10 related notes per category
- Max 5 KB per note (encourages brevity)
- Prioritize by date (newer first)
- Search by keyword if >10 results

### Session-Local Memory
Some memory is session-local (not persisted):
- Current task notes
- In-progress decision docs
- Temporary working notes

Only save to persistent memory when:
- Feature is approved and merged
- Bug is fixed and closed
- Pattern is validated across 2+ projects
- Decision is finalized

## Memory Search Strategy

### Finding Relevant Memory
Use these patterns when prompting agents:

**Search for bug pattern:**
```
Load memory: /incidents/ keyword="token"
(Find all token-related bugs)
```

**Search for related decision:**
```
Load memory: /decisions/ keyword="oauth"
(Find all OAuth-related decisions)
```

**Search for patterns:**
```
Load memory: /patterns/ keyword="async"
(Find all async patterns)
```

### Organizing Search Results
Results should be sorted by:
1. **Relevance** (how closely matches query)
2. **Recency** (newer first)
3. **Impact** (high-impact findings first)

## Memory Cleanup & Archival

### Cleanup Schedule
- **Weekly**: Mark stale notes as archived
- **Monthly**: Review deprecated items
- **Quarterly**: Deep cleanup (remove duplicates, consolidate)

### Archival Rules
When to archive memory:
- Decision is replaced by newer decision
- Pattern is no longer used in codebase
- Bug is fixed and no longer relevant
- Workflow optimization becomes standard

Archive format:
```
# [Original Title] [ARCHIVED: YYYY-MM-DD]

Reason: [Why archived?]
See instead: [Link to replacement]

---
## Original Content
[... original memory ...]
```

### Retention Policy
- **Active**: Currently used in development (indefinite retention)
- **Cached**: Referenced occasionally (1-year retention)
- **Archived**: No longer relevant (2-year retention, then delete)

## Integration with Hooks

### Memory Save Points
Hooks automatically save findings:

| Hook | Saves To | Format |
|---|---|---|
| post-impact.memory-snapshot | decisions/DATE-impact.md | Impact analysis |
| post-implementation.context-update | decisions/DATE-implementation.md | Implementation decisions |
| post-review.findings-archive | incidents/DATE-findings.md | Code review findings |
| post-debug.memory-archive | incidents/DATE-bug.md | Root cause analysis |
| context.update-patterns | patterns/*.md | New patterns discovered |
| post-pr.changelog-update | workflows/pr-changelog.md | Deployment notes |

### Memory-Aware Implementation
When implementing, agents check memory first:
```
Task: Implement payment retry logic

Pre-check memory:
✓ Found: incidents/2026-04-15-payment-retry-deadlock.md
✓ Found: patterns/exponential-backoff-pattern.md
✓ Found: decisions/2026-03-20-retry-strategy.md

Use these insights to avoid repeating past mistakes
```

## Memory Quotas

### Total Memory Budget
- Core system memory: 500 KB (architecture, workflow definitions)
- Pattern library: 200 KB (max 50 patterns)
- Decision archive: 300 KB (max 100 decisions)
- Incident archive: 400 KB (max 200 incidents)
- Workflow notes: 100 KB (max 20 workflows)

### Per-Note Limits
- Bug analysis: 2 KB max
- ADR: 1 KB max
- Pattern: 1 KB max
- Workflow optimization: 0.5 KB max

Enforce with: If note exceeds limit, split into multiple notes + link them.

## Memory Usage in Prompts

### Good Memory Citation
```
User: "Implement payment timeout"

Prompt:
Load spec: specs/payment-timeout.md
Load memory: incidents/2026-04-15-timeout-issue.md
Load memory: patterns/timeout-retry-pattern.md

Implement payment timeout feature with these constraints...
```

### Bad Memory Citation
```
User: "Implement payment timeout"

Prompt:
Implement feature. Here's entire memory archive [4 KB of text]...

(This wastes tokens and clutters context)
```

## Validation Rules

### Memory Quality Checklist
Before saving memory, verify:
- ✓ Concise (unnecessary details removed)
- ✓ Searchable (keyword-friendly title + content)
- ✓ Actionable (specific recommendations or lessons)
- ✓ Dated (current date added)
- ✓ Linked (related notes cross-referenced)
- ✓ Non-redundant (doesn't duplicate existing notes)

### Validation Tool
See `hooks/context/update-patterns.prompt.md` for memory quality validation prompt.
