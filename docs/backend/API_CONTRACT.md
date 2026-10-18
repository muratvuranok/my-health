# API contract v1 — implementation checklist
All private routes require authenticated subject and server-derived ownership; client-supplied profile IDs never confer authorization. Use JSON UTF-8, ISO-8601 UTC instants, explicit IANA timezone, UUID IDs, request_id, stable error codes, cursor pagination, If-Match/version or idempotency key for mutations.

## Users and profiles
GET/PATCH /v1/me; GET/POST /v1/profiles; GET/PATCH/DELETE /v1/profiles/{profileId}; GET /v1/me/sessions; POST /v1/me/export; DELETE /v1/me. Family grants model reserved but cross-account access disabled in MVP.

## Medication plans and dose events
GET/POST /v1/profiles/{profileId}/medications/plans; GET/PATCH/DELETE /v1/profiles/{profileId}/medications/plans/{planId}; GET/POST /v1/profiles/{profileId}/medications/plans/{planId}/events. Required: medication display name, dose text, schedule timezone, recurrence, start/end, provenance, version; event statuses taken/skipped/missed, occurred_at, occurrence_key and idempotency key. 409 for conflicting schedule changes; no automatic dose modifications.

## Measurements
GET/POST /v1/profiles/{profileId}/measurements; GET/DELETE /v1/profiles/{profileId}/measurements/{measurementId}; type blood_pressure/heart_rate/weight/temperature/spo2 (extensible), measured_at, value/unit, source, external ID, provenance, dedupe. Blood pressure has separate systolic and diastolic values.

## Consents and document metadata
GET/POST /v1/profiles/{profileId}/consents; GET/POST/DELETE /v1/profiles/{profileId}/documents. MVP document endpoints store synthetic metadata only; no unapproved real uploads or OCR. Consent purpose/version and withdrawal are immutable audit events.

## Sync
POST /v1/sync/push {device_id,mutations:[{id,resource,base_version,payload}]}; GET /v1/sync/pull?cursor=&limit=. Conflict safety, replay idempotency and monotonic cursors tested.

## Service
GET /v1/health/live, GET /v1/health/ready, GET /v1/openapi.json (local-only optional), GET /v1/version.

For each operation: generate OpenAPI 3.1 schemas, examples, error variants 400/401/403/404/409/422/429/500, authorization tests and curl usage. Never leak whether another user's resource exists (prefer consistent 404).
