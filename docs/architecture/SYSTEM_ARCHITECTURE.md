Status: SUPERSEDED (backend topology)
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: ADR-012; mobile assumption pending ADR-005

**Historical architecture only.** Go modular-monolith topology was superseded by accepted ADR-012. The SwiftUI-only assumption is under review in PROPOSED ADR-005. No implementation permission.

# Architecture — proposed, not implementation approval

- HISTORICAL iOS direction: SwiftUI, modular medication domain, encrypted offline-first persistence, local notifications, HealthKit adapter.
- HISTORICAL backend topology: Go modular monolith, PostgreSQL, worker for safe async tasks, versioned APIs.
- Sync: authenticated, idempotent mutation IDs, conflict detection, explicit schedule authority and provenance.
- Data: user/profile, medication plan and schedule version, dose event, measurement, document metadata, consent, device source and audit record.
- Local-first read path; sync optional and recoverable. Encryption-at-rest and E2EE versus server-readable choice remains OPEN.
- Future adapters: Health Connect, device BLE, manufacturer SDK/API, FHIR, country-specific records.

Never infer medical meaning or recommend dose changes from sensor readings. Device data requires units, measurement time, origin, calibration/validation status when known and deduplication keys.
