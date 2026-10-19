Status: ACCEPTED — implementation stack chosen; native risk gates OPEN
Date: 2026-10-18
Owner: My Health MASTER
Owner acceptance: 2026-10-19 — explicit owner choice "Mobil platform: React Native + Expo" in My Health decision message.
Supersedes: None
Superseded by: None

# ADR-005 — Cross-platform mobile client

## Context
Historical SwiftUI-only direction conflicts with the emerging React Native plan. Türkiye and Azerbaijan Android market-share estimates in the review are **to be verified** with dated independent statistics. HealthKit is iOS-only; Health Connect is Android-specific.

## Accepted choice and boundary
React Native + TypeScript and Expo development builds, with iOS/Android support and audited native Swift/Kotlin integrations for health permissions, background reminders, encrypted storage and OAuth. Native behavior must be validated on real devices; Expo Go is insufficient as an integration acceptance test.

## Alternatives
SwiftUI-only (deep Apple integration but excludes Android), separate SwiftUI/Kotlin apps (high duplication), Flutter (single UI framework with native bridges).

## Decision status and verification gates
**ACCEPTED:** React Native + TypeScript, Expo **Development Builds** (not Expo Go for native feature acceptance), cross-platform iOS/Android. Swift/Kotlin native adapters can be developed for platform-critical HealthKit/Health Connect, medication reminders, security/storage and future BLE. This is stack selection, **NOT** proof that SDK permissions, encrypted storage, reminders or background activity already work. Validate supported versions, legal permissions, native module maintenance, locked-device and multi-day reminder resilience on actual devices before live-data release. A separate synthetic-only prototype may use mocked adapters, explicitly labeled as mocks.

## Reversibility / change evidence
Changing after native adapters, navigation and data storage mature is expensive. Reverse if a realistic proof-of-concept exposes missing critical OS capability or a materially better supported alternative.
