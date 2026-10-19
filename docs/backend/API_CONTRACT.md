Status: DRAFT — NOT FROZEN
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# API contract v1 — implementation checklist
All private routes require authenticated subject and server-derived ownership; client-supplied profile IDs never confer authorization. Use JSON UTF-8 with **snake_case** wire field names, ISO-8601 UTC instants, explicit IANA timezone, UUID IDs, request_id, stable error codes, cursor pagination, If-Match/version or idempotency key for mutations.

## Users and profiles
GET/PATCH /v1/me; GET/POST /v1/profiles; GET/PATCH/DELETE /v1/profiles/{profileId}; GET /v1/me/sessions; POST /v1/me/export; DELETE /v1/me. Family grants model reserved but cross-account access disabled in MVP.

## Medication plans and dose events
GET/POST /v1/profiles/{profileId}/medications/plans; GET/PATCH /v1/profiles/{profileId}/medications/plans/{planId}; PROPOSED POST /v1/profiles/{profileId}/medications/plans/{planId}/archive (no hard delete); GET/POST /v1/profiles/{profileId}/medications/plans/{planId}/events. Required: medication display name, dose text, schedule timezone, recurrence, start/end, provenance, version; recorded event statuses taken/skipped/snoozed; missed is a derived view state (PROPOSED); scheduled_for, taken_at (when applicable), occurred_at, occurrence_key and idempotency key. 409 for conflicting schedule changes; no automatic dose modifications.

## Measurements
GET/POST /v1/profiles/{profileId}/measurements; GET/DELETE /v1/profiles/{profileId}/measurements/{measurementId} (PROPOSED soft-delete plus sync tombstone); type blood_pressure/heart_rate/weight/temperature/spo2 (extensible), measured_at, value/unit, source, external ID, provenance, dedupe. Blood pressure has separate systolic and diastolic values.

## Consents and document metadata
GET/POST /v1/profiles/{profileId}/consents; **Deferred — not in MVP (PROPOSED, pending owner):** GET/POST/DELETE /v1/profiles/{profileId}/documents. No misleading metadata-only upload promise; no real uploads/OCR. Likewise /v1/devices is deferred. Consent purpose/version and withdrawal are immutable audit events.

## Sync
POST /v1/sync/push {device_id,mutations:[typed mutation oneOf by resource, mutation_id, base_version, payload]} (PROPOSED; schema not frozen); GET /v1/sync/pull?cursor=&limit=. Conflict safety, replay idempotency and monotonic cursors tested.

## Integration and messaging extensions (DRAFT — NOT FROZEN)
- Integration capabilities, per-profile source import preferences, typed measurement import batches and disconnect: see [INTEGRATION_API_DRAFT.md](INTEGRATION_API_DRAFT.md) and accepted architectural boundary [ADR-016](../decisions/ADR-016-INTEGRATIONS-PERMISSIONS.md).
- Notification preferences, push-device registration and optional in-app message center: see [NOTIFICATION_API_DRAFT.md](NOTIFICATION_API_DRAFT.md), accepted design [ADR-015](../decisions/ADR-015-NOTIFICATIONS-MESSAGING.md).
- No implicit HealthKit/Health Connect grants through the backend; device-local OS grants and server cloud-import consent are distinct. No endpoint schema is frozen.

## Service
GET /v1/health/live, GET /v1/health/ready, GET /v1/openapi.json (local-only optional), GET /v1/version.

For each operation: generate OpenAPI 3.1 schemas, examples, error variants 400/401/403/404/409/422/429/500, authorization tests and curl usage. Never leak whether another user's resource exists (prefer consistent 404).

## Proposed contract amendments (D2–D6, B8)
- Archive/end medication plans; preserve historical plan versions and adherence events. Soft delete measurements with tombstones for offline clients.
- Distinguish recorded dose actions from derived missed/late states; scheduled_for and actual taken_at are different concepts.
- Use explicit oneOf/discriminator mutation schemas, user/profile-scoped opaque cursors, revocation behavior, conflict and partial batch result contracts.
- Server returns stable machine error codes and request_id; mobile client owns tr/az/en translations. Errors must never disclose whether a resource exists in another account.
- Structured optional dose quantity as decimal string + coded unit + form; measurements as exact decimal strings and reviewed UCUM-compatible units (proposal, not frozen).
- No changed endpoint semantics should ship until owner approves and an OpenAPI 3.1 contract + compatibility tests exist.
