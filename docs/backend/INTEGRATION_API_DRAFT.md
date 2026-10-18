Status: DRAFT — integration endpoint and data schemas NOT FROZEN
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Integration API contract — design proposal

## Scope and protocol
Only public My Health APIs for a signed-in user's device-originating health data and integration preferences. Direct OS HealthKit/Health Connect grants cannot be requested by the backend; mobile native adapters obtain them from OS. No server-based OAuth connection for HealthKit or Health Connect. This is a **candidate endpoint list and bodies**, not approved executable OpenAPI; wire convention ADR-007, architecture ADR-016.

## Suggested resources
- `integration_source`: per installation and OS adapter, not a globally linked account.
- `import_policy`: what source/types a user opted to import to which authorized profile and whether cloud upload is enabled.
- `import_batch`: source cursor and bounded measurement ingestion idempotency.
- `import_result`: server-confirmed accepted/duplicate/rejected/conflict counts and typed item outcomes.
- `integration_connection`: later OAuth/device partner connectors; NOT used to pretend platform HealthKit grants.

All `source_system` enums: `apple_healthkit`, `android_health_connect`, `manual`, future vendor-specific additions. No provider plaintext OAuth/access token in any response. `profile_id` is never authorization.

## GET /v1/me/integration-capabilities
200 example:
```json
{"data":{"sources":[{"source_system":"apple_healthkit","connection_model":"device_permission","data_types":["blood_pressure","heart_rate","weight","body_temperature","oxygen_saturation"],"write_supported":false,"requires_os_permission":true},{"source_system":"android_health_connect","connection_model":"device_permission","data_types":["blood_pressure","heart_rate","weight","body_temperature","oxygen_saturation"],"write_supported":false,"requires_os_permission":true}]},"request_id":"req_example"}
```
Capabilities are app/server supported types, **not** proof that any OS grant is available or that user's handset supports either platform. Native adapter must intersect actual capabilities.

## PUT /v1/profiles/{profile_id}/integration-policies/{source_system}
Headers `If-Match: "v1"` or creation `Idempotency-Key` — precise versioning semantics TBD.
```json
{"installation_id":"opaque_installation_id","enabled_types":["weight","heart_rate"],"sync_to_cloud":false,"import_from":"2026-10-01T00:00:00Z","retention_choice":"until_disconnected","purpose":"personal_health_tracking"}
```
200/201 example:
```json
{"data":{"policy_id":"policy_uuid","version":2,"source_system":"apple_healthkit","enabled_types":["weight","heart_rate"],"sync_to_cloud":false,"state":"enabled","updated_at":"2026-10-18T10:00:00Z"},"request_id":"req_example"}
```
**OPEN**: whether `sync_to_cloud=false` policy belongs locally only (avoids sending unnecessary consent metadata to server), retention choices, permissions for managed profiles, semantics under E2EE/local-only ADRs. Do not assume device permission from this write.

## POST /v1/profiles/{profile_id}/measurement-imports
Headers: `Idempotency-Key: <opaque UUID>`.
```json
{"source_system":"android_health_connect","installation_id":"opaque_installation_id","batch_id":"uuid","schema_version":1,"items":[{"client_mutation_id":"uuid","source_record_id":"opaque-native-sample-id","source_type":"weight","record":{"type":"weight","measured_at":"2026-10-18T09:30:00Z","components":[{"code":"body_weight","value_decimal":"70.50","unit_code":"kg"}],"provenance":{"source_application":"com.example.sensordata"}}}]}
```
202 candidate:
```json
{"data":{"batch_id":"uuid","status":"queued","accepted_items":1,"duplicate_items":0,"rejected_items":0,"results":[],"operation_id":"uuid"},"request_id":"req_example"}
```
Important: **202 queued cannot truthfully claim accepted_items** unless server validated and persisted outcomes synchronously; choose either 200/207 style per-item finalized result or 202 with only `operation_id`+poll resource. Batch atomicity and partial successes must be decided under ADR-010/ADR-007. Maximum batch size, decimal precision, source identity and replay retention OPEN. This example is a draft showing an issue, NOT a frozen contract.

## GET /v1/profiles/{profile_id}/measurement-imports/{batch_id}
200 draft:
```json
{"data":{"batch_id":"uuid","status":"completed","summary":{"inserted":1,"duplicates":0,"rejected":0},"results":[{"client_mutation_id":"uuid","status":"inserted","measurement_id":"uuid"}]},"request_id":"req_example"}
```
No unbounded result lists; pagination or bounded batches needed. Results must not leak another account's source identifiers.

## POST /v1/profiles/{profile_id}/integration-policies/{policy_id}/disconnect
Request proposed `{"stop_future_imports":true,"delete_imported_copies":false}`.
202 proposed with `operation_id`; request must not claim it revoked HealthKit/Health Connect OS grants. Return UX guidance to OS settings, and domain-owner workflow for erasure if requested. Define cancellation of in-flight imports, tombstones, backup retention and legal exceptions.

## Normalized measurement semantics
Use exact decimal strings with unit codes; BP separate systolic/diastolic and provenance, no auto-merge with manual records. `measured_at`, `received_at`, `source_record_id`, `source_application`, `source_type`, `device_id?`, `is_deleted` for source deletion, and `adapter_schema_version`. Scope idempotency and dedup by profile+source+application+source ID. Sensitive source IDs hashed or encrypted where appropriate. Validate value formats but do not diagnose or make medication recommendations.

## Error taxonomy (candidate)
`INTEGRATION_UNSUPPORTED`, `INTEGRATION_NOT_CONFIGURED`, `PERMISSION_REQUIRED` (device-side only; cannot infer HealthKit read denial from server), `DATA_TYPE_UNSUPPORTED`, `IMPORT_CURSOR_EXPIRED` (device-side), `CONSENT_REQUIRED`, `PROFILE_NOT_OWNED` (externally mask as 404), `IMPORT_CONFLICT`, `IMPORT_PARTIAL_FAILURE`, `SOURCE_RATE_LIMITED`, `SOURCE_UNAVAILABLE`, `SOURCE_REVOKED`, `SOURCE_VERSION_UNSUPPORTED`. Each code must have explicit mapped HTTP status and safe client-localized message before freeze.

## Review gates
Real-device permission and entitlement proofs; backend authorized profile checks; deleted and edited source records; cross-OS unit mapping; batch replay; source cursor loss; mid-batch permission revoked; deletion/disconnect and backup; event propagation; health-data-free logs; schema examples and generated client compile; migration and contract diff. **OpenAPI 3.1 and approval are still required.**
