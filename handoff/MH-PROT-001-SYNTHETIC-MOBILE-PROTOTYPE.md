Status: READY FOR OWNER IMPLEMENTATION GO — planning handoff only
Date: 2026-10-19
Owner: My Health MASTER
Supersedes: Historical MH-001/MH-002 planning instructions for prototype
Superseded by: None

# MH-PROT-001 — Synthetic mobile prototype handoff

## Role and authority
For Claude working with Lili AI on My Health only. The project owner approved six architectural and scope decisions on 2026-10-19. This document authorizes **preparation of the implementation plan**, not an unlimited coding instruction. Actual MH-PROT-001 coding requires a separate owner GO. The old full backend handoff remains SUSPENDED.

## Required reading
MASTER.md, TASKS.md, docs/product/MVP_SCOPE.md, docs/testing/RELEASE_GATES.md, SECURITY.md, ADR-001–004, accepted ADR-005, ADR-007 (conventions only), ADR-012, ADR-014, ADR-017, open ADR-009 and ADR-010. ADR-015/016 are proposals, not approved technical implementations.

## Objective
Plan an offline-first React Native + TypeScript / Expo mobile prototype using **only invented deterministic records**. It should demonstrate initial personal health tracking and an **on-device caregiver-managed profile** without real accounts, web APIs or third-party integrations.

## Intended clickable demo
1. Onboarding: local language selection and prominent demo-data label, no real registration.
2. Profile switcher: personal profile and separately identified caregiver-managed profile, clearly isolated. No family invitations or linked accounts.
3. Medication: fictitious medication plans, a limited illustrative daily schedule, displayed timezone, daily timeline, plan editing and explicit taken/skipped logging. Retain historical records and distinguish planned vs recorded time. Do not claim medical-grade recurrence or dosage authority.
4. Measurements: invented weight and blood pressure samples, exact units, date/time and synthetic source.
5. Reminder experience: generic private preview and simulated UI states only; do not claim reliable OS notification scheduling without a later native-device test.
6. Export preview: visibly marked demo, with no real clinical report or user health information.
7. Professional, Nutrition, Fitness, messaging and billing modules remain clearly labeled BACKLOG/unavailable. No professional entity/schema may enter the MVP through this demo.

## Technical bounds
- Accepted mobile choice: React Native + TypeScript, Expo Development Builds for any later native adapters; no proof of HealthKit/Health Connect from Expo Go.
- Use local repository interfaces with test fakes, deterministic fixtures, clock injection and profile-scoped state. Server request/response shapes are NOT frozen, and local TypeScript interfaces must not be presented as final OpenAPI.
- Accepted ADR-012 Go microservices + Kubernetes remains the eventual backend approach, but **no backend, K8s deployment or cloud sync is necessary for this local-only prototype**.
- Storage encryption and real-device background notifications are not proven by a mocked demo. Mark such capabilities NOT VERIFIED.
- Do not implement production authentication, email/push providers, health record integrations, medical decision logic, actual professional sharing or device BLE.

## Prototype acceptance criteria for a later implementation PR
- Reproducible app startup, documented SDK versions and commands; synthetic screens visually verified in the supported simulator.
- Test personal versus managed-profile data isolation, schedule display, separate scheduled/actual times, immutable historical logs, measurements/units, offline demo behavior and fake data source.
- No hidden network dependency; no real person identifiers or health data. No diagnosis, treatment recommendation or automatic dose adjustment.
- Generic reminder privacy preview; pending iOS/Android scheduling and background access marked untested.
- Basic tr/az/en localization scaffolding, accessible controls and clear client-facing demo disclaimers.
- Automated tests/type checks and actual results recorded. No fabricated device, legal, security or production testing claims.
- Feature branch and PR with screens, test evidence, limitations and human review. No merge without MASTER acceptance.

## Gates still open
ADR-009 medication time rules, ADR-010 server encryption and sync, field-level OpenAPI contract, real native HealthKit/Health Connect access, reliable OS alarms, secure health-data local storage, backup/restore, country privacy/law and all real-user release gates. None are waived.

## Handoff output expected now
A small implementation plan with architecture interfaces, file tree, fake fixture schema, test cases and estimated native dependencies. Record any blockers, then request explicit scoped MH-PROT-001 implementation GO. Do not execute the suspended Claude backend task.
