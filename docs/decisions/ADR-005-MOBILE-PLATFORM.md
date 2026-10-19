Status: PROPOSED — owner approval required
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-005 — Cross-platform mobile client

## Context
Historical SwiftUI-only direction conflicts with the emerging React Native plan. Türkiye and Azerbaijan Android market-share estimates in the review are **to be verified** with dated independent statistics. HealthKit is iOS-only; Health Connect is Android-specific.

## Proposal
React Native + TypeScript and Expo development builds, with iOS/Android support and audited native Swift/Kotlin integrations for health permissions, background reminders, encrypted storage and OAuth. Native behavior must be validated on real devices; Expo Go is insufficient as an integration acceptance test.

## Alternatives
SwiftUI-only (deep Apple integration but excludes Android), separate SwiftUI/Kotlin apps (high duplication), Flutter (single UI framework with native bridges).

## Decision status
**PROPOSED**. No framework or first-launch platform selection is authorized by this ADR yet. Acceptance requires capability matrix, dependency maturity/licenses, OS support, actual notification/HealthKit/Health Connect tests, staffing and estimated migration cost.

## Reversibility / change evidence
Changing after native adapters, navigation and data storage mature is expensive. Reverse if a realistic proof-of-concept exposes missing critical OS capability or a materially better supported alternative.
