Status: ACCEPTED (implementation details OPEN)
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-012 — Microservices and Kubernetes learning-first architecture
Date: 2026-10-18
Status: ACCEPTED IN PRINCIPLE; implementation details and contract freeze OPEN
Decision owner: My Health MASTER

## Decision
Adopt a deliberately small, independently deployable microservice architecture for My Health, and use Kubernetes as the target orchestration platform. A major goal is hands-on learning transferable to other projects. This supersedes the earlier modular-monolith *deployment topology* recommendation, but retains Go/PostgreSQL, API-first contracts, synthetic-only local development and all safety gates. No automatic production or real-health-data GO.

## Initial logical boundaries (subject to domain review)
1. API gateway/BFF: external HTTP surface, auth enforcement, quotas, version routing, aggregation; does not own clinical data.
2. Identity: OAuth/OIDC (Google and Apple), email/password, magic link, sessions, identity links.
3. Profile/access: users' health profiles, ownership and future family grants. Identity owns authentication identity; profile owns domain profile.
4. Medication: medication plans, recurrence versions, dose events, safety invariants.
5. Health records: measurements, provenance, document metadata; actual file storage gated.
6. Sync: orchestration/protocol, conflict outcomes, cursors; does NOT own or directly modify other services' tables.
7. Notification: async email delivery and event-driven notification processing; mobile scheduled reminders remain on device.

Do not force one Kubernetes Deployment per conceptual module before boundary validation. Start with 4–6 deployable services where ownership and transactional consistency justify them; worker may share codebase with owning service. Each service has own schema/database, credentials, migrations and least-privilege access. A single local Postgres instance can host separate databases. No cross-service SQL joins, foreign keys or shared writes. Avoid distributed transactions; use transactional outbox, idempotent consumers, retries and compensating actions where necessary. Service ownership and event schemas must be documented.

## Platform plan
- Local phase A: Docker Compose + service-specific health/readiness, metrics and tests.
- Local phase B: k3d or kind cluster; namespace, ConfigMaps, local Secrets (dummy only), Deployments, Services, Gateway/Ingress, probes, resource requests/limits, network policies where supported, volumes, Job migrations, rollout/rollback.
- Deployment packaging: Helm chart or Kustomize, choose one in an ADR and test it. Pin images by digest for release. GitHub Actions build/test/scan; no automatic production deploy.
- Observability: OpenTelemetry traces, metrics and structured logs with health-data redaction; local Prometheus/Grafana optional after baseline.
- No service mesh, Kafka, complex autoscaling or managed cloud Kubernetes in MVP unless demonstrated need.

## Contract-first guardrails
External REST/OpenAPI 3.1 and generated React Native TypeScript client. Internal service communication default HTTP/JSON for simplicity; consider gRPC only if measured need. Publish ownership, authentication propagation, service identity, event versioning, idempotency, retry budgets, deadlines, error taxonomy and backwards compatibility rules before code. API gateway is not an authorization substitute; every data-owning service enforces authorization. Avoid shared user tables and distributed joins. Medication schedule and event operations remain strongly consistent within medication boundary.

## Mandatory architecture questions
- Identity vs profile boundary: registration lifecycle, eventual consistency, deletion and re-registration.
- Medication vs sync boundary: schedule edits, conflict ownership and offline replay.
- Health records vs files: storage metadata, blobs, retention and encryption.
- User deletion and consent revocation across services: orchestration, tombstones, audit and eventual completion.
- Security of service-to-service calls: authenticated workload identities or signed short-lived service tokens; never trust forwarded user_id headers.
- Schema/event migrations and N-1 compatibility during rolling deploys.
- Whether sync warrants separate deployment at first; only split if clear operational ownership.

## Kubernetes learning curriculum
K0: container images, Compose, Postgres migration and local E2E.
K1: Pods, Deployments, Services, DNS, kubectl, namespace.
K2: probes, ConfigMaps, Secrets, resource budgets, logs, port-forward.
K3: Ingress/Gateway, TLS local, Helm/Kustomize, rollout/rollback.
K4: autoscaling basics, observability, backup/restore, NetworkPolicy, RBAC.
K5: CI artifact promotion, supply chain, cluster upgrade, incident drills.
Every module must include reproducible lab instructions, deliberate failure scenario and recovery evidence. Do not expose a public cluster or use real secrets/health data.

## Quality gates
Cross-service contract tests, isolated DB migration tests, authz/tenant isolation, event replay and duplicate delivery, timeout/partial outage resilience, service unavailable and rolling deployment tests, Kubernetes manifest validation, security scans, local k3d/kind E2E, documented recovery. The contract remains NOT FROZEN until field-level schemas and independent review.

## Impact on existing documents
MASTER.md, SYSTEM_ARCHITECTURE.md, BACKEND_MASTER_PLAN_V1.md and CLAUDE_BACKEND_EXECUTION.md were written for modular monolith and now contain outdated assumptions. This ADR takes precedence for topology only. Do not start the earlier one-binary end-to-end coding order unchanged; first prepare reconciled microservice plan and contracts. The user's earlier local synthetic-data scope remains; any new production actions require GO.

## Open issues raised in review 2026-10-18
- [B1] PROPOSED ADR-013 suggests modular-monolith-first deployment plus k3d training; **does not override accepted microservices/Kubernetes strategy**.
- [B2] Define who applies sync mutations when Sync cannot access Medication-owned data: synchronous service calls vs per-resource endpoints; map domain 409 and version headers through BFF; specify mixed batch success, idempotency, atomicity scope and timeout retries before freezing contract.
- [B9] Rate limits across Kubernetes replicas require shared durable/distributed enforcement; investigate PostgreSQL-backed limits vs Redis. 'No Redis initially' cannot imply isolated per-replica security limits.
- [B4] On-device reminders may stop being scheduled when mobile OS quotas/background limits apply; notification-service assumptions must not imply server push guarantees; see RELEASE_GATES.md.
