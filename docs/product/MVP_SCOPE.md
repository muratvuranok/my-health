Status: DRAFT (under revision)
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# MVP scope and acceptance

## P0
1. Mobile app using **ACCEPTED ADR-005**: React Native + TypeScript, Expo Development Builds, iOS and Android target; Turkish, Azerbaijani and English localization. Swift/Kotlin adapters for OS-critical functionality when needed.
2. Medication list, clinician-verified schedule entry, dosage event history, offline local notifications, explicit confirmation and missed/skipped states.
3. HealthKit permission flow and read-only ingestion of selected vital measurements with source/provenance and timestamps.
4. Encrypted local storage for eventual real-data use; **initial alpha is local-only under ACCEPTED ADR-014**. Account creation, cloud sync, server encryption and real backend communication are not required for initial alpha and remain separately gated.
5. Daily timeline, basic export for clinician discussion, accessibility and error states.
6. Privacy settings, data export/deletion requests and consent controls.
7. Automated tests, real-device verification, backup/restore, threat model and private TestFlight gate.

## Prototype vs alpha
**First authorized planning target:** synthetic-only, entirely local interactive prototype using deterministic invented records. This does not certify notification delivery, HealthKit, Health Connect, encrypted storage or privacy. **Initial alpha strategy:** local-only, with no cloud account/sync/provider calls, per ADR-014. Any transition to real personal health data needs GATE-00/01/02 plus applicable storage/privacy/device evidence and explicit MASTER GO; GATE-03/04 remain in force for later cloud sync / real-data beta where applicable.

## Accepted MVP scope clarification: on-device caregiver-managed profile (E3)
The owner explicitly chose **YES — a caregiver-managed profile on the same device** on 2026-10-19. A local operator may create and switch between a personal profile and a managed dependent profile for synthetic alpha demonstration, with clear person labels and strict separation of medication plans and events. This is **not** cross-account access, invitations, clinical sharing, staff access or professional workspaces. Use lawful authority/consent and applicable guardian/privacy validation **before real dependent data**. Do not add grants, workspace billing or third-party relationship APIs to MVP.

## Explicit exclusions
Full web dashboard, watchOS, family collaboration beyond architecture, e-Nabız live sync, direct BLE pairing, AI diagnosis, Arduino blood-pressure hardware.

## Acceptance
No duplicate/incorrect scheduled doses after timezone or daylight saving changes; reminders work offline; conflicts are visible, not silently resolved; HealthKit never overwrites medication authority; sensitive content excluded from analytics/logs; deletion/recovery verified.

## Proposed additions — pending owner approval
- [B4] Design/test reminder continuation when iOS pending notifications reach the commonly cited 64 ceiling, the app remains unopened for days, and Android Doze/exact-alarm policies apply; verify platform specifics and avoid claiming guaranteed background execution.
- [B6] Default to generic lock-screen reminders without medication names; opt-in disclosure per profile.
- [E3] **DECIDED YES:** same-device caregiver-managed profile is in MVP product scope (see above); third-party grants/cross-account sharing remain deferred under ADR-001.
