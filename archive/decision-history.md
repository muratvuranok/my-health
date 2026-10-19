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

- **2026-10-19 — ADR-005 — ACCEPTED (native direction):** Owner explicitly abandoned React Native in chat after evaluating native development. Mobile architecture is Swift/SwiftUI for iOS and Kotlin/Jetpack Compose for Android; first release order and specific libraries remain OPEN. No source code GO implied.
