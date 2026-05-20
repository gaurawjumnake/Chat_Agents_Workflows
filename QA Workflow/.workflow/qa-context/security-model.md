# Security Validation Model

## Validation Domains

- Authentication and token/session handling
- RBAC and authorization boundaries
- API abuse resistance (rate, payload, misuse)
- Dependency risk and vulnerable package drift
- Secret exposure and config leakage
- Threat model delta for changed surfaces

## Required Evidence

- Auth and RBAC test outputs
- Negative and abuse API test outputs
- Dependency audit summary
- Secret scanning summary
- Threat model update note

## Gate Rule

`pre-release.security-validation` must return pass before release readiness can pass.
