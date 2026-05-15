# API Test Matrix

## Columns

- Endpoint
- Method
- Auth required
- RBAC role expectations
- Contract tests
- Negative tests
- Abuse tests
- Backward compatibility checks

## Matrix Rules

- Every changed endpoint must include contract and negative tests.
- Security-sensitive endpoints must include abuse-path tests.
- Breaking changes must include migration validation.
