Status: PROPOSED — integration boundary design; provider configurations and endpoint schemas OPEN
Date: 2026-10-18
Owner: My Health MASTER
Owner acceptance: NOT RECORDED — no explicit owner approval of this ADR has been established; requires a separate owner decision.
Supersedes: None
Superseded by: None

# ADR-016 — Integration architecture, authorization and data contracts

## Decision and scope
**All external integrations use capability-specific adapters and an explicit registry; no vendor SDK is allowed to directly mutate medication schedules or write cross-service databases.** The integration boundary is logical, not necessarily a new deployed microservice. React Native orchestrates user consent and local platform adapter use. Go services own authorization, data provenance, durable sync, device registry and auditable server side provider connections. The medication service retains authority over schedule rules (ADR-003). This ADR proposes **integration architecture conventions**, NOT blanket health access, a provider partnership, live OAuth credentials, any write permission, or final endpoint field sets.

## Integration registry
| ID | Capability / provider | Owner | Proposed launch direction | Permission / credential | Direction | Rules |
|---|---|---|---|---|---|---|
| INT-01 | Apple HealthKit | iOS native adapter + Records | Read-only selected measurements | Per-type HealthKit read; HealthKit entitlement and usage text | HealthKit -> local records -> optional approved sync | No write grants initially; no medication plan overwrite |
| INT-02 | Android Health Connect | Android native adapter + Records | Read-only selected measurements | Manifest + user-granted per-type READ permissions | Health Connect -> local records -> optional approved sync | Background/history extra permission only with documented necessity |
| INT-03 | Google Sign-In | Identity service | Account authentication | OIDC openid/email/profile, PKCE/state/nonce as applicable | Provider -> Identity | Do not request Gmail mailbox scopes; never auto-link by email |
| INT-04 | Sign in with Apple | Identity service | Account authentication | Apple sign-in capability and provider identity; private relay support | Provider -> Identity | Stable issuer/sub key, token validation; no HealthKit permission implied |
| INT-05 | Email sender (SMTP/API) | Identity + Notification worker | Transactional email | Configured sender domain/provider key, no user mailbox permission | My Health -> recipient | Mailpit local; production provider OPEN |
| INT-06 | APNs/FCM | Notification + mobile adapters | Remote push for informational/service messages | iOS notification authorization and APNs registration; Android notification runtime permission where required and FCM token | Server -> device | Best-effort, no PHI in payload and not canonical medication reminder |
| INT-07 | Local reminders | Mobile / Medication rules | Offline first | OS local notification permission and platform scheduling limits | App -> device | No server dependency, no on-time guarantee |
| INT-08 | Device BLE/manufacturer SDK | Future integration adapter | DEFERRED | Explicit discovery/connection, provider scopes, vendor agreements | Device -> Records | Exact model/protocol accuracy and legal validation required |
| INT-09 | Country medical records (e-Nabız / AZ systems) | Future regulated integration | DEFERRED | Official program and legally supported scopes only | Provider -> Records | Official API availability, license, consent, residency TO BE VERIFIED |
| INT-10 | FHIR/HL7 partner interface | Future server adapter | DEFERRED | OAuth scopes, partner trust and FHIR profile | Partner <-> controlled resources | No blanket permission; schema version and privacy assessment required |

## Patient-data types — minimal launch request matrix
| Internal type | Proposed HealthKit type | Proposed Health Connect record | Default access | Medical semantics |
|---|---|---|---|---|
| blood_pressure | HKQuantityType bloodPressureSystolic / bloodPressureDiastolic | BloodPressureRecord | read after user opts into BP import | Paired measurements with method/body position when available; no diagnosis |
| heart_rate | heartRate | HeartRateRecord | read after user opts in | Device-supplied series may contain multiple samples; preserve time and metadata |
| weight | bodyMass | WeightRecord | read after user opts in | Preserve original unit and normalized display representation |
| body_temperature | bodyTemperature | BodyTemperatureRecord | read after user opts in | Preserve unit, site/metadata where supported |
| oxygen_saturation | oxygenSaturation | OxygenSaturationRecord | read after user opts in | Map fractions/percent correctly, verify SDK format |
These mappings are **implementation candidates; check current official SDK availability, regional/OS support and data type identifiers before native coding.** Do NOT request menstrual health, genetic data, medical records, medications, location, contacts, photos or other scopes speculatively. Neither platform's permission implies the user's authorization to upload data to our server or grant access to another account.

## Three-layer access policy
1. **Operating system permission:** Platform-owned grant per data type or notification capability. Ask only after a specific in-app action; handle denied, restricted, limited history, revoked, OS unavailable and later settings changes. On HealthKit, read access denial is deliberately not fully observable: empty result is not proof of denial or absence. Do not display a fabricated 'granted' read status.
2. **Application purpose / privacy preference:** Per-account profile, source, category, sync direction, server-upload opt-in, retention and visibility. The profile whose data is imported must be legally and technically authorized; a caregiver cannot assume a parent's HealthKit data is accessible from a phone profile without explicit authority.
3. **Server authorization / grants:** Server-derived account, actor, subject/profile and purpose. Explicit future family grants are independent from OS and upload consent. Tokens and capability claims from the device never confer profile ownership.

## Mobile adapter boundary and contract
`HealthSourceAdapter` responsibilities: `isAvailable()`; `supportedTypes()`; `requestReadAccess(types[])`; `readIncremental(cursor, type, limit)`; `observeChanges()` where platform allows; `getPermissionUXState()` without claiming invisible HealthKit read grants; `disconnect()` local consent/prefs only. Return normalized typed `HealthSample` and source-specific cursor/deletion markers with adapter version and query window. Native Swift/Kotlin bridge implementations are reviewed separately; avoid coupling business logic to a third-party library API.

**Per sample fields (candidate, NOT frozen):**
`source_system`, `source_record_id`, `source_application`, `source_device_id?`, `source_type`, `type`, `measurement_components[{code,value_decimal,unit_code}]`, `measured_at`, `measured_end_at?`, `received_at`, `offset_minutes?`, `timezone?`, `provenance_metadata?`, `external_version?`, `import_batch_id`, `is_deleted`, `schema_version`.
Deduplicate on `(account/profile, source_system, source_application, source_record_id, source_type)` with correction/version rules; do not rely solely on timestamps. Store source-native identifiers encrypted/least-privilege; do not include raw health measurements in telemetry or SDK analytics. Android and Apple removal/deletion cursors must be processed; expired cursors cause bounded rescan, never silent truncation.

## Sync / data access behavior
- First import: explicit user-selected categories, date window and upload preference; progress/cancel; no implicit full-history backfill.
- Incremental: persisted per-device/source/type cursor with OS-specific fetch semantics, page caps and retry state. No claim of exactly-once delivery.
- Import creates a **measurement record** with original source and units; does not change medication plans; if identical measurement exists from multiple sources, retain provenance and present dedup/conflict policy rather than silent clinical merge.
- Source update/deletion: match source record; create tombstone/correction event; do not silently erase independent user-entered records; user-controlled deletion and legal erasure policy OPEN.
- Import paused/revoked: stop further reads/writes, invalidate pending jobs, offer local/cloud deletion choices consistent with applicable law and ADR-010. OS revoke cannot retroactively erase copies; product must expose controls.
- Unsupported platform/type: explicit disabled UX and manual entry path; no fake test pass.
- Health source is the *phone owner's* protected store, not automatically the health data of caregiver-managed family profiles.

## Server-side integration state machine
Potential future server connectors: `not_configured -> consent_pending -> authorization_pending -> connected -> degraded|reauthorization_required -> disconnected|revoked`. Error states distinguish permission denied, unsupported device/OS, rate limited, expired token, refresh failure, policy/region block and source deleted. Client-owned HealthKit/Health Connect do **not** create false server OAuth connections; model as local source authorizations and sync preferences. Tokens and refresh credentials are stored only with appropriate platform secrets / backend secret storage, never in event payloads or chat.

## Security & operational controls
Least-privilege scope/entitlement; purpose description localized tr/az/en; never over-ask on onboarding. Google/Apple auth has no mailbox/health scopes. Separate auth tokens from health authorization and push tokens. Encrypt local sensitive data, minimize retention, honor user-initiated disconnect and consent withdrawal. Document privacy disclosure, Apple/Google store declarations, provider agreements, breach impact, residency/transfers, and production legal basis **before live data**. Queue only minimal opaque references, no health payload. Provider error logs redacted; rate limit with shared state under K8s.

## Test matrix / release gates
At least: permission approve/deny/revoke mid-sync; partial/limited history; no data vs denied; missing capability; Health Connect version differences and permission declaration; HealthKit authorized query with zero results; device locked/background tasks; duplicated and corrected sample; deleted record/tombstone; timezone/units; reconnect with expired cursor; offline long delay; parallel devices with same sample; wrong-profile selection; managed-profile vs linked-account access; consent revoked during import; full account erasure; source/mapping upgrade; OAuth invalid issuer/aud/nonce/PKCE; provider token expiry; notification permission denial; APNs/FCM token rotation; email bounce and rate limits; third-party outage; data export sanitization; PHI-free logs. Synthetic fixtures first; live real-device tests only under applicable gates.

## Official platform references (to re-verify at implementation)
- https://developer.apple.com/documentation/healthkit/authorizing-access-to-health-data — fine-grained HealthKit read/share; read denial not directly observable.
- https://developer.android.com/health-and-fitness/health-connect/data-types — per-type permissions and optional history/background read permissions.
- https://developer.android.com/health-and-fitness/health-connect/get-started — manifest and Play-related access setup.
- https://developer.apple.com/documentation/usernotifications/asking-permission-to-use-notifications — context-based notification permission.
- https://firebase.google.com/docs/cloud-messaging/android/get-started — Android 13+ notification runtime authorization.
- https://developer.android.com/develop/connectivity/bluetooth/bt-permissions — Android BLE scan/connect permissions.
- https://docs.expo.dev/workflow/customizing/ — development builds and custom native Swift/Kotlin modules.
All claims tied to provider policies must be rechecked at implementation/store submission; dated market, legal and per-model support are not decided by this ADR.

## Change control and open decisions
No blanket HealthKit or Health Connect write access. Future writeback, background/history reads, partner/FHIR/e-Nabız, BLE, cloud upload defaults and retention need additional owner + privacy/regulatory gates. Native adapter package/vendor choice, permitted history window, data type support, sync timing, encryption key model, consent UX, exact request/response bodies and per-source retention are **OPEN**. Reversing the adapter boundary after production adoption is expensive; revise only using capability proof and migration/contract tests.
