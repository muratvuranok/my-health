# MY HEALTH — ARCHITECTURE & API CONTRACT AUDIT V1
Date: 2026-10-18. Status: REVIEW IN PROGRESS / CONTRACT NOT FROZEN.
Scope: Mobile iOS+Android, Go API, PostgreSQL, auth, medication/measurements, sync, privacy, testing, migration. This is an evidence-backed first audit, not a claim of exhaustive global research or tested implementation.

## Decision: hold contract freeze and implementation that would harden provisional interfaces
Existing docs/backend/API_CONTRACT.md and AUTH_DESIGN.md are endpoint outlines, NOT field-complete executable OpenAPI. The prior implementation GO must not be interpreted as permission to freeze speculative payloads or silently decide unresolved encryption, family sharing or health safety rules. Existing local synthetic prototype work may continue only when independent of these decisions. Full API implementation should wait for approved OpenAPI and contract test suite.

## External reference review (2026-10-18)
- https://github.com/gin-gonic/gin — mature, popular Go HTTP framework; higher framework coupling than chi.
- https://github.com/go-chi/chi — idiomatic net/http router; good modular monolith candidate.
- https://github.com/zeromicro/go-zero — microservices and layered request lifecycle; learn timeouts, rate limiting, code generation, not mandatory framework adoption.
- https://github.com/go-kratos/kratos — API-first protobuf HTTP/gRPC microservices; avoid premature distribution.
- https://github.com/labstack/echo — maintained net/http-compatible alternative.
- https://github.com/oasdiff/oasdiff — breaking-change detection for OpenAPI 3.1; use in CI.
- https://www.rfc-editor.org/rfc/rfc9700.html — OAuth 2.0 Security BCP: PKCE, redirect matching, state/nonce, mix-up defenses.
- https://github.com/expo/expo — Expo development builds for native modules.
- https://github.com/appeeky/expo-healthkit — illustrative community HealthKit/Health Connect bridge; verify maintenance, native coverage, entitlements, privacy and device behavior before selecting.
No star count alone determines framework choice.

## Proposed technical baseline (pending ADR approval)
React Native + TypeScript + Expo development builds, native Swift/Kotlin adapters; Go modular monolith with net/http+chi; pgx+sqlc; PostgreSQL 16+; Goose migrations; Postgres transactional outbox; OpenAPI 3.1 first, generated typed TypeScript client; Testcontainers-Go; Mailpit; Docker Compose; GitHub Actions; oasdiff + OpenAPI lint; slog + OpenTelemetry with strict PHI suppression. Version pins, licenses, platform OS matrix and supply-chain assessment required before freeze. Avoid unnecessary Redis/Kafka/microservices.

## Contract governance: mandatory before coding public interfaces
1. Define OpenAPI 3.1 schema per operation including auth, path/query/header/body, required/optional/null distinctions, max sizes, formats, enums, defaults, examples, response codes and retry semantics.
2. Stable error envelope with machine code, request_id, safe message and field violations; define 400 vs 401 vs 403 vs 404 vs 409 vs 412 vs 422 vs 429; avoid leaking resource existence.
3. Explicit resource ID semantics: UUID/opaque IDs, ownership, actor vs subject, immutable external source IDs; never identify Google/Apple user by email alone.
4. Pagination: cursor opaque and scoped to filters/sort, deterministic order and deletion behavior; no unbounded lists.
5. Dates: UTC instants vs local date vs local wall time vs IANA timezone; DST nonexistent/ambiguous times; recurring schedules and timezone travel policy; no floating-point doses, weight or monetary values without unit/precision model.
6. HTTP: PATCH semantics (JSON Merge Patch vs field mask), optimistic version/ETag, If-Match/412, idempotency-key and replay TTL, 201 Location, 202 async operation resources, 204 no body, stable caching rules.
7. JSON: unknown field policy, nullable vs absent, enum evolution/UNKNOWN, number precision, snake_case/camelCase convention, consistent arrays, explicit defaults and backwards-compatible field addition.
8. Authentication: OAuth code+PKCE, nonce/state, issuer/audience, key rotation; Apple relay email and first-time name; magic link email scanners and POST confirmation; account linking collision prevention; rotating refresh reuse detection; account deletion and re-registration policy.
9. Medication: separate catalog identity, user-entered free text, clinician verification metadata (do not imply doctor verification when self-entered), plan versions, schedules, occurrence IDs, adherence events vs actual administration; edits never silently mutate history. Multiple meds, paused/resumed, PRN, tapering, DST, travel, missed/late, duplicate device events and cross-user grants.
10. Measurements: blood pressure systolic/diastolic/pulse with units, source, measured_at vs received_at, device provenance, correction/deletion, dedupe and provenance; measurement ranges validated without diagnosing.
11. Documents: metadata vs blob, storage keys never exposed, encryption and malware scanning design, immutable versions, retention, signed URL TTL; no real uploads until gated.
12. Sync: monotonic cursor, tombstones, server reconciliation, retry/out-of-order, clock skew, duplicate mutations, authorization revocation, account deletion, conflict policy per resource, multi-device refresh.
13. Privacy: field classification and log/trace redaction; encryption at rest vs E2EE unresolved; country residency and cross-border transfer; subject access/export/deletion and backup retention.
14. Localization: Turkish/Azerbaijani/English, Unicode normalization, case-folding email without unsafe assumptions, units, calendar, accessibility, error localization.
15. API lifecycle: /v1 stable, additive evolution rules, deprecation window policy, oasdiff CI against main, generated clients compile/test, fixtures and snapshot tests. No compatibility promises before contract approval.

## Mandatory risk register (initial)
CRITICAL — Email signup/verification and magic-link token response ambiguity: generic 202 must not leak account status; token never in GET-triggered auto-login.
CRITICAL — Existing medication model insufficient for PRN, tapering, DST, travel, historical corrections and verified-vs-self-entered plans.
CRITICAL — Sync encryption choice OPEN, affecting server-side conflict detection and exports.
HIGH — Family access revocation and multi-profile authorization semantics not yet specified.
HIGH — Document blobs excluded but endpoints could misleadingly promise uploads.
HIGH — React Native native health adapters need actual iOS/Android entitlement/device proof.
HIGH — OAuth provider configuration and Apple private relay cannot be proven in local simulator.
HIGH — Account deletion/backups/retention not contractually defined.
HIGH — Numeric precision and time models under-specified.
HIGH — API breaking-change enforcement not implemented.

## Required deliverables before CONTRACT FREEZE
A. ADR-005 mobile platform choice and OS support matrix.
B. ADR-006 Go HTTP/SQL/migration/testing stack with pinned versions.
C. ADR-007 API contract conventions and compatibility policy.
D. ADR-008 auth/session/account linking state machines.
E. ADR-009 medication domain and recurrence semantics.
F. ADR-010 sync/conflict/encryption decision (requires MASTER).
G. ADR-011 consent/family authorization/data retention.
H. OpenAPI 3.1 full schemas and example fixtures for ALL endpoints.
I. Traceability matrix: endpoint -> domain invariant -> test -> security threat -> acceptance.
J. CI workflow with schema lint, oasdiff breaking check, generated SDK compile, DB migration verification, unit/integration/E2E/security.
K. Architecture decision review by independent reviewer, and explicit MASTER contract freeze GO.

## Test design requirements
Contract/schema positive/negative tests; property-based schedule/DST tests; Testcontainers PostgreSQL transactions/ownership tests; concurrent refresh/magic redemption; fuzz request parsers; mutation/replay/reorder/offline multi-device E2E; migration clean/upgrade/rollback on disposable DB; iOS/Android real device HealthKit/Health Connect permission tests; chaos restart/recovery; performance baseline with p95, error rate and DB saturation under representative synthetic load. Record real results, never invent pass. Test coverage target is heuristic, not sufficient evidence of correctness.

## Recommended next work
Conduct focused architecture review and contract-design PR before broad API coding. Deliver decision log and explicit open questions that truly require business/regulatory authority. No changes to production, health records or other repositories.
