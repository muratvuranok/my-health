Status: DRAFT — local acceptance plan, not execution GO
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# One-command local delivery and acceptance checklist

## Expected commands
- cp .env.example .env (synthetic values only)
- make dev-up (or ./scripts/dev-up.sh) builds and starts PostgreSQL, API, worker, Mailpit
- make migrate-up, make seed-demo
- make test, make test-integration, make test-security, make test-e2e
- make smoke prints concise PASS/FAIL and non-sensitive evidence
- make dev-down
All commands must be documented and actually executed on the target environment. Support macOS/Linux Docker; document Windows PowerShell equivalents or provide cross-platform Make/Docker instructions.

## E2E smoke narrative
1. Readiness/DB migration PASS.
2. Email signup request, view verification in Mailpit, verify and authenticate.
3. Password login and invalid password tests.
4. Magic link request; Mailpit displays link; GET does not consume; explicit POST consumes; reuse rejected.
5. Google and Apple dev-simulator identities authenticate only when APP_ENV=local and simulator enabled. Negative production-mode startup test proves simulator unavailable.
6. Create a synthetic profile and medication plan; record taken/skipped events; duplicate event idempotent; invalid/unauthorized resource rejected.
7. Create synthetic blood-pressure measurement with unit/source; list and paginate.
8. Offline sync mutation replay idempotent; conflicting medication edit returns 409.
9. Export synthetic account data, request deletion, verify token invalidation and deletion behavior.
10. Restart containers and prove persistence, run migrations from clean DB, run rollback in disposable DB, confirm logs contain no tokens or PHI.

## Quality bar
go test ./..., race tests on critical modules, integration tests with real Postgres, negative auth/ownership tests, coverage report (target 80% critical auth/domain branches; explain exceptions), lint/static analysis, OpenAPI contract validation, reproducible build and documented known gaps. No fake PASS for provider live integration or compliance. Every test result must be real.

## Handoff
One report: exact commit/PR, tree, prerequisites, commands, observed output, E2E matrix, unresolved provider credentials/encryption decision, security risks, decisions and scope exclusions. No intermediate user approvals for normal coding steps. Escalate only genuine blocked actions outside local/synthetic scope.
