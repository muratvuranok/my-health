Status: ACCEPTED — native mobile development; platform release order OPEN
Date: 2026-10-19
Owner: My Health MASTER
Owner acceptance: 2026-10-19 — owner chat explicitly withdrew React Native for mobile ("react vazgeçtim, mobilde") following native-versus-React Native discussion; native iOS/Android is the resulting platform direction. Specific framework/library versions and iOS-versus-Android release sequencing remain OPEN.
Supersedes: ADR-005 earlier PROPOSED React Native/Expo recommendation; historical SwiftUI-only iOS assumption to the extent it excludes planned Android support
Superseded by: None

# ADR-005 — Native mobile clients for My Health

## Decision
My Health mobile applications will be developed **natively**:
- **iOS:** Swift + SwiftUI, with HealthKit, UserNotifications, Keychain, native device storage/security and optional Core Bluetooth integrations.
- **Android:** Kotlin + Jetpack Compose, with Health Connect, platform notification/alarms, Android Keystore, native encrypted storage and optional Bluetooth integrations.
- **Shared external interface:** contract-first, versioned REST/JSON OpenAPI 3.1 to the Go backend. Shared *wire contracts and business invariants*, not a mandatory shared mobile UI runtime.
- **No React Native or Expo as the planned mobile framework.** TypeScript generated clients mentioned in older documents are historical/proposed and no longer a normative mobile-platform requirement.

This decision covers platform architecture, not a GO to implement, release, ingest real medical data or approve field-level API contracts. The **first platform to prototype/release (iOS-first versus parallel iOS+Android)** remains OPEN; target markets still include both platforms.

## Rationale
Reliable, testable direct access to HealthKit and Health Connect, native notification scheduling and handling, background restrictions, encrypted local storage, platform-specific permission UX and future BLE integrations. Native APIs do not bypass iOS/Android scheduling or background restrictions: test on real devices.

## Trade-offs and engineering controls
- Two clients increase UI implementation, developer workload and ongoing platform maintenance.
- Avoid divergent behavior via shared OpenAPI 3.1 contracts, machine error codes, consistent state transitions, seeded synthetic fixtures, golden behavior tests and release gates. Swift/Kotlin generated SDKs may be used after contract freeze, not handwritten ad-hoc API variations.
- Keep platform-specific UI accessible and idiomatic, but user-facing rules for medication occurrence identity, dose history, versioning, idempotency, consent and sync must be specified independently and implemented/tested on both platforms.
- Record security/keychain/keystore constraints and permissions per OS version. Real-device verification is mandatory for notifications, offline behavior and health-data integration.

## Alternatives considered
React Native + native adapters/Expo development builds (now explicitly declined by owner), Flutter and SwiftUI iOS-only. Revisit only upon explicit owner amendment and comparative evidence; do not silently introduce RN as a convenience dependency.

## Open choices
First prototype and public release order; minimum iOS/Android OS versions; supported device list; state management/navigation/DI/UI testing libraries; encrypted database technology and recovery model under ADR-010; platform permission feature matrix; CI build/signing and distribution (ADR-004 remains binding).

## Change cost
Switching UI platform after release is expensive; require explicit decision and migration/reliability evidence. Accepted ADR-012 microservices/Kubernetes backend architecture and ADR-017 one My Health product/account experience remain unchanged: one *product* may have distinct native iOS and Android binaries.
