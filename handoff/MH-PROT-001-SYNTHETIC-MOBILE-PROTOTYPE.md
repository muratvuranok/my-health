Status: PLAN ONLY — parallel native iOS + Android platform decision recorded; explicit implementation GO required
Date: 2026-10-19
Owner: My Health MASTER
Supersedes: Historical MH-001/MH-002 planning instructions for prototype
Superseded by: None

# MH-PROT-001 — Synthetic mobile prototype handoff

## Role and authority
For Claude working with Lili AI on My Health only. The project owner recorded initial scope decisions on 2026-10-19, then **reversed the same-day React Native acceptance in favor of native iOS/Android**. **Owner platform decision (2026-10-19): BOTH native iOS and Android prototypes will be planned in parallel.** This document authorizes **preparation of the implementation plan**, not an unlimited coding instruction. Actual MH-PROT-001 coding requires a separate owner GO. The old full backend handoff remains SUSPENDED.

## Required reading
MASTER.md, TASKS.md, docs/product/MVP_SCOPE.md, docs/testing/RELEASE_GATES.md, SECURITY.md, ADR-001–004, accepted ADR-005, ADR-007 (conventions only), ADR-012, ADR-014, ADR-017, open ADR-009 and ADR-010. ADR-015/016 are proposals, not approved technical implementations.

## Objective
Plan an offline-first **native Swift/SwiftUI iOS and/or Kotlin/Jetpack Compose Android** mobile prototype using **only invented deterministic records**. The owner selected **parallel iOS (Swift/SwiftUI) and Android (Kotlin/Jetpack Compose)** prototype scope on 2026-10-19. This remains a planning-only handoff; development requires a **separate explicit implementation GO**. The future production release sequence remains OPEN. It should demonstrate initial personal health tracking and an **on-device caregiver-managed profile** without real accounts, web APIs or third-party integrations.

## Intended clickable demo
1. Onboarding: local language selection and prominent demo-data label, no real registration.
2. Profile switcher: personal profile and separately identified caregiver-managed profile, clearly isolated. No family invitations or linked accounts.
3. Medication: fictitious medication plans, a limited illustrative daily schedule, displayed timezone, daily timeline, plan editing and explicit taken/skipped logging. Retain historical records and distinguish planned vs recorded time. Do not claim medical-grade recurrence or dosage authority.
4. Measurements: invented weight and blood pressure samples, exact units, date/time and synthetic source.
5. Reminder experience: generic private preview and simulated UI states only; do not claim reliable OS notification scheduling without a later native-device test.
6. Export preview: visibly marked demo, with no real clinical report or user health information.
7. Professional, Nutrition, Fitness, messaging and billing modules remain clearly labeled BACKLOG/unavailable. No professional entity/schema may enter the MVP through this demo.

## Technical bounds
- Accepted mobile choice: separate native **Swift/SwiftUI (iOS)** and **Kotlin/Jetpack Compose (Android)** clients per ADR-005. Do not use React Native, Expo or TypeScript for the mobile prototype. No live HealthKit/Health Connect integration is approved.
- Use **native Swift protocols / Kotlin interfaces** for locally scoped ProfileRepository, MedicationPlanRepository, DoseEventRepository and MeasurementRepository abstractions with fakes; deterministic fixture loading, injected clock/time source and profile-scoped state. These local abstractions are NOT approved backend request/response schemas.
- Store a **platform-independent JSON fixture specification** (stable fixture IDs, profile_id, version, synthetic medication plans, scheduled_for, taken_at, event identity, measurement value_decimal and unit_code) so both native clients can consume the same test cases. Fixture records contain invented, non-identifying values only.
- Apply identical behavior expectations to both platform implementations while permitting idiomatic SwiftUI/Compose UI differences. Native app development and the external agent orchestration project are separate responsibilities.
- Accepted ADR-012 Go microservices + Kubernetes remains the eventual backend approach, but **no backend, K8s deployment or cloud sync is necessary for this local-only prototype**.
- Storage encryption and real-device background notifications are not proven by a mocked demo. Mark such capabilities NOT VERIFIED.
- Do not implement production authentication, email/push providers, health record integrations, medical decision logic, actual professional sharing or device BLE.

## Prototype acceptance criteria for a later implementation PR
- Reproducible native app startup on **both owner-selected native platforms**, documented Xcode/Swift or Android SDK/Gradle versions and commands; synthetic screens visually verified in the matching simulator/emulator. Neither platform may be marked DONE without its own build/test evidence; differences must be documented, not silently hidden.
- Test personal versus managed-profile data isolation, schedule display, separate scheduled/actual times, immutable historical logs, measurements/units, offline demo behavior and fake data source.
- No hidden network dependency; no real person identifiers or health data. No diagnosis, treatment recommendation or automatic dose adjustment.
- Generic reminder privacy preview; pending iOS/Android scheduling and background access marked untested.
- Basic tr/az/en localization scaffolding, accessible controls and clear client-facing demo disclaimers.
- Native unit/UI tests (XCTest/Swift Testing where supported and/or Kotlin/JUnit/Compose testing), deterministic fake clock, JSON fixture parity tests and actual build/test results recorded. No fabricated device, legal, security or production testing claims.
- Feature branch and PR with screens, test evidence, limitations and human review. No merge without MASTER acceptance.

## Gates still open
ADR-009 medication time rules, ADR-010 server encryption and sync, field-level OpenAPI contract, real native HealthKit/Health Connect access, reliable OS alarms, secure health-data local storage, backup/restore, country privacy/law and all real-user release gates. None are waived.

## Handoff output expected now
Two coordinated **native platform-specific** implementation plans with Swift protocols **and** Kotlin interfaces, project/file tree, **shared language-agnostic JSON** fake-fixture schema, deterministic-clock tests, device/OS matrix, per-platform tests, parity criteria and estimated Xcode/Android native dependencies. Record **OWNER DECISION SATISFIED: both iOS and Android in parallel (2026-10-19)** and keep this handoff **PLAN ONLY** until the owner grants separate scoped MH-PROT-001 implementation GO for implementation. Do not execute the suspended Claude backend task.
