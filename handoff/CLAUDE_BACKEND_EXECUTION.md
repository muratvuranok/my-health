# CLAUDE CODE — MY HEALTH BACKEND END-TO-END EXECUTION ORDER
Repository: muratvuranok/my-health (private)
Task: MH-BE-001
Read first, in order: MASTER.md, SECURITY.md, docs/product/MVP_SCOPE.md, docs/architecture/SYSTEM_ARCHITECTURE.md, docs/backend/BACKEND_MASTER_PLAN_V1.md, docs/backend/AUTH_DESIGN.md, docs/backend/API_CONTRACT.md, docs/backend/LOCAL_ACCEPTANCE.md, docs/testing/RELEASE_GATES.md.

## Authorization
GO for implementation and local execution with **synthetic data only**. Complete the full Go API, PostgreSQL, worker, Mailpit, Docker Compose, auth and health domain API, OpenAPI, migrations, tests and smoke scripts. Make reasonable reversible technical choices and document them; do not ask for incremental approval or repeatedly stop for progress checkpoints. Implement independent work even if a production-only dependency is blocked.

## Security/release restrictions
Do not use real personal/health data, private chat transcripts, live provider credentials, production infrastructure, cloud spend, deployment, account creation at external services, or changes to other repositories. Do not bypass safety controls. No direct main push/merge; use feature branch and PR. Never invent OAuth secrets. Provider simulation must be development-only and fail closed outside local mode. Do not claim actual Apple/Google provider sign-in validated without real credentials. Respect open encryption ADR: do not silently decide production cloud key access. No clinical diagnosis or dose recommendation.

## Done means
Real local build and running containers; migrations applied; API endpoints actually callable; auth negative tests and account isolation; complete synthetic CRUD and safe sync; docs/OpenAPI; deterministic seed; end-to-end smoke PASS; final report with commands/results and PR. If environment cannot run Docker, implement as much as possible, provide exact limitation and do not claim end-to-end PASS.

## Communication
Do not interrupt for normal architecture choices or per-endpoint confirmations. Report only genuine external blockers or boundary-crossing decisions. Deliver one consolidated completion report. Task is not CLOSED until independent review and MASTER acceptance.
