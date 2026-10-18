Status: ACCEPTED — conventions only; operation schemas NOT FROZEN
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-007 — External API conventions and compatibility policy

## Decision
My Health adopts **contract-first HTTP REST/JSON** for mobile-facing versioned APIs, with OpenAPI 3.1 as the versioned source of truth before endpoint implementation. This approves cross-cutting conventions, **not** every operation's payload schema. API freeze and production readiness require endpoint-specific review.

## Wire format
- UTF-8 JSON; **snake_case** field keys. Disallow duplicate JSON keys and unknown fields on security-critical write operations (document the exact policy in OpenAPI). Consistent null vs omitted vs empty semantics per schema.
- Opaque public IDs (UUID recommended; no database sequence exposed); RFC3339 UTC instants with explicit offset, IANA timezone for recurring user schedules; local date and time are distinct typed values.
- Exact decimal values represented as decimal strings for medication/measurement precision where relevant; unit codes and provenance required by domain rules still OPEN under ADR-009.
- Stable machine error envelope proposed: `{"error":{"code":"invalid_field","request_id":"...","fields":[{"field":"...","code":"required"}]}}`. No sensitive info or user enumeration; client renders localized tr/az/en messages. Field set and errors require schema tests.
- Consistent 200 read/update, 201 created + Location, 202 accepted async, 204 successful no-body, 400 parse/shape, 401 authentication, 403 authorization when safe, 404 non-disclosing resource absence, 409 domain conflict, 412 failed If-Match, 422 semantic invalidity, 429 rate limit; each endpoint documents a finite subset. Retry-After for throttling where appropriate.
- Pagination uses opaque cursor scoped to subject, filter and ordering; deterministic order, tombstone/change handling. Never unlimited multi-profile records.
- Use If-Match/ETag optimistic concurrency for mutable resources and Idempotency-Key for create/action replays. Spell out duration, scope, replay outcomes and status semantics per operation; do not promise exactly-once provider delivery.
- Avoid boolean fields that later become multi-state without migration. Enums specify how older clients handle unknown values. No silent server defaults for medication dosing or safety-critical time semantics.

## Compatibility governance
External `/v1` remains stable for clients already released. Add optional response fields compatibly; do not repurpose types/semantics or make previously optional request fields mandatory without versioning/negotiation. OpenAPI lint, oasdiff breaking-change review, generated TypeScript client compile and test fixtures required in CI before release. Service-internal contracts/events are separately versioned and rolling-deployment compatible (N-1 migration policy to define).

## Security and scope
Authentication/authorization checked at data owner, not just gateway; never trust client supplied profile owner, event actor or service identity. Explicit upload size limits, rate limits shared across replicas, TLS, restricted CORS, PHI-free observability and retention apply across APIs.

## Remaining OPEN
Precise errors/body examples, required/nullable semantics, request sizes, PATCH field mask vs JSON Merge Patch, access-token representation, idempotency retention window, cursor invalidation behavior, notification endpoint names, sync batch atomicity, provider-specific health permissions and E2EE impact. Do not infer these from generic conventions.

## Reversal and evidence
Changing public conventions after mobile release is costly; version and provide compatibility tests. Revisit only for demonstrated platform/protocol incompatibility or approved schema migration with contract-diff review.
