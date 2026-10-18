# MVP scope and acceptance

## P0
1. Native SwiftUI iOS app; Turkish, Azerbaijani and English localization.
2. Medication list, clinician-verified schedule entry, dosage event history, offline local notifications, explicit confirmation and missed/skipped states.
3. HealthKit permission flow and read-only ingestion of selected vital measurements with source/provenance and timestamps.
4. Encrypted local vault and secure account/sync once encryption architecture approved.
5. Daily timeline, basic export for clinician discussion, accessibility and error states.
6. Privacy settings, data export/deletion requests and consent controls.
7. Automated tests, real-device verification, backup/restore, threat model and private TestFlight gate.

## Prototype vs alpha
Prototype uses synthetic data, no actual medical decisions. Alpha with real data requires MASTER GATE-00 through GATE-04 approval.

## Explicit exclusions
Full web dashboard, watchOS, family collaboration beyond architecture, e-Nabız live sync, direct BLE pairing, AI diagnosis, Arduino blood-pressure hardware.

## Acceptance
No duplicate/incorrect scheduled doses after timezone or daylight saving changes; reminders work offline; conflicts are visible, not silently resolved; HealthKit never overwrites medication authority; sensitive content excluded from analytics/logs; deletion/recovery verified.
