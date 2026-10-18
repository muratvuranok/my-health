# Architecture — proposed, not implementation approval

- iOS: SwiftUI, modular medication domain, encrypted offline-first persistence, local notifications, HealthKit adapter.
- Backend: Go modular monolith, PostgreSQL, worker for safe async tasks, versioned APIs.
- Sync: authenticated, idempotent mutation IDs, conflict detection, explicit schedule authority and provenance.
- Data: user/profile, medication plan and schedule version, dose event, measurement, document metadata, consent, device source and audit record.
- Local-first read path; sync optional and recoverable. Encryption-at-rest and E2EE versus server-readable choice remains OPEN.
- Future adapters: Health Connect, device BLE, manufacturer SDK/API, FHIR, country-specific records.

Never infer medical meaning or recommend dose changes from sensor readings. Device data requires units, measurement time, origin, calibration/validation status when known and deduplication keys.
