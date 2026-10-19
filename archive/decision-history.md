Status: DRAFT — historical record
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Decision history

2026-10-18 — Working brand My Health; global-ready product, Türkiye/Azerbaijan initial validation (STR-001). Lili AI orchestrates implementation, with strict repo separation. First delivery is synthetic-data iOS prototype, followed by gated TestFlight MVP. Device connectivity is post-MVP (MH-IOT-01). ADR-001–004 retained. Encryption mode and web launch timing remain OPEN.

## Dated decisions and proposals (historical status, not execution authorization)
- **2026-10-18 — ADR-012 — ACCEPTED IN PRINCIPLE:** Owner instructed use of a bounded microservice/Kubernetes learning-first architecture; topology supersedes original monolith plan. Deployment details and API contracts remain OPEN. Source: owner chat and ADR-012.
- **2026-10-18 — ADR-007 — ACCEPTED (conventions only):** Owner requested approval of current architecture/API-contract documentation in chat. HTTP/OpenAPI 3.1 conventions accepted; field-level schema freeze NOT approved. See Owner acceptance statement in ADR-007.
- **2026-10-18 — ADR-015 — PROPOSED:** Notifications, push and transactional email architecture authored by AI; no standalone owner acceptance established in the reviewed history. Release and payloads OPEN.
- **2026-10-18 — ADR-016 — PROPOSED:** Integration permission and adapter architecture authored by AI; no standalone owner acceptance established in the reviewed history. Provider rights and endpoint contracts OPEN.
- **2026-10-18 — ADR-017 — ACCEPTED (structural only):** Owner explicitly specified one My Health product and identity system, purpose-based onboarding and module-conditioned experience in chat. Professional-workspace commercialization, patient sharing, schemas and MVP expansion NOT approved. Interview evidence requirement remains unmet.
- **2026-10-18 — ADR-018 — PROPOSED:** Optional paid modules, subscriptions and entitlement packaging explored in chat, not adopted as commercial or pricing decision; no feature release GO.
- **2026-10-18 — ADR-019 — PROPOSED:** In-app patient/professional messaging explored in chat; privacy, retention, encryption and clinical release authorization OPEN.
- **2026-10-18 — FUTURE-001/002/003 — PROPOSED / BACKLOG:** Coach, professional care team and nutrition meal plans are future research hypotheses, explicitly excluded from current MVP data model until interviews and owner GO.
- **2026-10-19 — PR #21 round 2 review:** R1–R9 document reconciliation requested. Records/statuses above are scoped to evidence, not inferred from prior AI decisions. ADR-005/009/010/013/014 and hosting/brand/pricing remain OPEN or PROPOSED.

## Owner decisions — 2026-10-19 (explicit user statement)
- **ADR-005 ACCEPTED:** React Native + Expo for mobile (interpreted as Expo Development Builds + TypeScript with native Swift/Kotlin bridges as needed). Platform capability validation remains OPEN.
- **ADR-012 AFFIRMED; ADR-013 NOT SELECTED:** deploy architecture remains bounded Go microservices + Kubernetes; no modular-monolith-first amendment.
- **ADR-014 ACCEPTED FOR INITIAL ALPHA ONLY:** begin with local-only alpha; ADR-002 longer-term controlled cloud sync unchanged. Real-data gates not waived.
- **MVP caregiver-managed profile YES:** on one device, with separate managed profile, not cross-account grants; legal/consent validation before real dependent data.
- **ADR-017 ACCEPTANCE REAFFIRMED:** one app and account architecture, purpose-based future module contexts; professional modules not in MVP.
- **ADR-010 REMAINS OPEN:** no server-readable KMS vs E2EE selection, no hosting/provider selection.
- **Task intent:** update ADR/MASTER/TASKS accordingly, preserve unresolved API and security gates, and **prepare** the first synthetic mobile prototype handoff; no blanket GO to execute previously suspended backend work.

- **2026-10-19 (later the same day) — ADR-005 REVISED / ACCEPTED NATIVE:** Owner explicitly withdrew the earlier React Native/Expo selection recorded in main commit `4d9d20b` and chose native **Swift/SwiftUI for iOS and Kotlin/Jetpack Compose for Android**. The previous RN acceptance above is retained as historical evidence, not current implementation authority. ADR-005 now supersedes it. The first prototype platform (iOS / Android / both) remains OPEN; no agent setup or coding authorization is implied.
