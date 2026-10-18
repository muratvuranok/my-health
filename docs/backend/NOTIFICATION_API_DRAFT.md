Status: DRAFT — proposed request/response contracts; NOT FROZEN
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Notification API and event contracts — v1 design draft

All client JSON keys snake_case, RFC 3339 UTC timestamps, IANA timezones, opaque IDs/UUIDs; versioned OpenAPI 3.1 must be authored before any implementation GO. `Authorization: Bearer <token>` is illustrative; exact session model OPEN. All endpoints enforce server-resolved owner/profile grants. Return stable machine `code`, `request_id`, `details`; localization belongs to clients. Do not expose target addresses or device push tokens to other users.

## Resource: notification preference
Suggested model `{category, channel, enabled, profile_id?, quiet_hours?:{start_local,end_local,timezone}, privacy_level:"generic"|"detailed", version}`.
Categories are typed enums and should permit future additive values; marketing must have separate consent. Quiet hours must specify DST handling; safety reminders may have different eligibility rules. If-Match required for updates; conflict `412 PRECONDITION_FAILED` is distinct from domain `409`.

### GET /v1/me/notification-preferences
**200** `{"data":{"items":[{"category":"account_security","channel":"email","enabled":true,"privacy_level":"generic","version":1}]},"request_id":"req_example"}` (synthetic)
**401** unauthorized; account only, no profile cross-access.

### PATCH /v1/me/notification-preferences/{category}/{channel}
Headers: `If-Match: "1"`, `Content-Type: application/json`.
Body: `{"enabled":false,"privacy_level":"generic"}` (example; may reject changes to mandatory security events).
**200** updated preference with next version, **400** invalid input, **403** forbidden/mandatory exception, **412** stale version, **429** abuse limiter. Exact patch/null convention to be decided in ADR-007/OpenAPI.

## Resource: device push registration
### POST /v1/me/push-devices
Body: `{"installation_id":"opaque-client-install-id","platform":"ios","push_token":"provider-token","permission_status":"authorized","app_version":"0.1.0"}`.
**201** `{"data":{"id":"device_uuid","installation_id":"opaque-client-install-id","platform":"ios","enabled":true,"created_at":"2026-10-18T10:00:00Z"},"request_id":"req_example"}` plus Location. Provider token never returned. Never trust client-declared permissions without platform checks. 400/401/409/422/429.
### DELETE /v1/me/push-devices/{device_id}
**204** after revocation (idempotent semantics under review). Must scope to user/device ownership; prune invalid tokens on provider feedback. Logout and account deletion must revoke tokens even when explicit DELETE never arrives.

## In-app message center (optional, release timing OPEN)
### GET /v1/me/notifications?cursor=&limit=
**200** `{"data":{"items":[{"id":"message_uuid","category":"account_security","created_at":"2026-10-18T10:00:00Z","read_at":null,"preview":"Security notice"}],"next_cursor":null},"request_id":"req_example"}`. Do not place diagnoses or medication names in default preview.
### POST /v1/me/notifications/{notification_id}/read
**204** mark read. This is NOT provider-delivered/read receipt and not medication taken evidence.

## Domain and internal events (not public HTTP)
Event envelope proposal: `{"event_id":"uuid","event_type":"medication.plan_changed.v1","aggregate_id":"uuid","aggregate_version":2,"occurred_at":"2026-10-18T10:00:00Z","actor_ref":"opaque","profile_ref":"opaque","payload_ref":"opaque"}`.
- Medication service emits plan-changed/cancelled events, never exposes raw patient content as broker payload.
- Identity emits verification/password/security lifecycle events for email delivery, preferably one-time, hashed token state in Identity not Notification.
- Notification consumes permitted minimal data through service-authenticated APIs, with authorization checked at dispatch and retry.
- Per-recipient `notification_intent_id`, `delivery_id`, `attempt_number`, `channel`, `status` (`queued|submitted|failed|cancelled`), `provider_reference` (redacted), `expires_at`. Provider `submitted` does not imply delivered or read.

## Open questions to decide before OpenAPI freeze
Endpoint path choice, external device ID format, push token retention, auth scheme, fields required/nullable, 202 asynchronous requests, maximum body sizes, batch operations, rate limits, preference precedence across profiles, quiet hours + DST, caregiver consent, mobile foreground/background behavior, security mail bypass, email template versions, provider callbacks, status enumeration and how to signal required transactional message failure. Also align /v1/me/export and DELETE /v1/me with all notification data erasure and audit retention rules.

## Required validation
OpenAPI schema positive/negative contract tests; ownership/IDOR; dedupe/retry/429; APNs/FCM token refresh; Mailpit sandbox; provider callbacks; revocation during in-flight retries; lock-screen leak tests, device-locked background access; notification count and Android exact alarm policy tests on supported physical devices.
