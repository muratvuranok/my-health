Status: DRAFT; v2 PROPOSED
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Product and startup roadmap — 12 months

All dates and numeric targets are planning hypotheses, not results.

| Period | Product | Commercial proof | Gate |
|---|---|---|---|
| Months 1–2 | native SwiftUI iOS + Kotlin/Compose Android synthetic prototypes in parallel (accepted ADR-005, MH-PROT-001), tr/az/en | 30–40 interviews across Türkiye/Azerbaijan; 10 usability tests | Problem/scope |
| Months 3–4 | Medication Core, HealthKit, encrypted local data, safe sync; TestFlight alpha | 20–50 opt-in pilot users | Safety, privacy, recovery |
| Months 5–6 | Limited family beta, basic reports, subscription experiments | 100–300 users and 20–50 paying users as hypotheses | Pricing and retention |
| Months 7–9 | Device feasibility and retention improvements | Cohort retention, CAC, churn, partner conversations | Product-market fit evidence |
| Months 10–12 | English/global readiness and investor data room | Select third-market experiment, unit economics | Scale/no-scale decision |

**Not in first MVP:** full web, standalone Apple Watch/watchOS or Wear OS apps, direct BLE integrations, e-Nabız live integration, AI medical diagnosis, custom medical device hardware.

## Proposed roadmap v2 (H, B3, E1, E3) — NOT APPROVED
1. Week 1: MASTER v1.3, suspend outdated handoff, ADR-005; return repo private only when owner decides.
2. Weeks 2–4: 30–40 interviews and caregiver-focused synthetic prototype.
3. Months 2–3: consider local-only mobile alpha, reliable reminders, HealthKit/Health Connect, export, caregiver-managed profile.
4. Parallel learning: evaluate one Go workload in k3d (PROPOSED ADR-013; ADR-012 remains accepted).
5. Month 4+: revisit hosting, encryption, API/sync and family beta with actual evidence.
Android market-share claims for TR/AZ are **to be verified** with dated evidence.

## Smartwatch apps — phased post-MVP direction (owner planning, 2026-10-19)
The owner wants **both Apple Watch/watchOS and Android Wear OS** considered in the My Health product roadmap, but developed **in later stages, not alongside MH-PROT-001**. This is scope sequencing only; it is **not an implementation GO**, an approved release date, or a requirement to build watch applications or agent infrastructure now.

| Stage | Delivery target | Scope and entry condition |
|---|---|---|
| 0 — Current | Native iOS + Android phone synthetic prototype (MH-PROT-001) | Both platforms in parallel; existing handoff and separate implementation GO remain binding. No watch code. |
| 1 — Phone foundation | Usable local-first medication/timeline and controlled real-device feasibility | Phone dose-state semantics, reminders, privacy and local state must be demonstrated before a watch companion can rely on them. |
| 2 — Companion watch apps (post-MVP) | Apple Watch app (SwiftUI/watchOS) and Wear OS app (Kotlin/Compose for Wear OS), staged per capacity | Minimal daily medication overview, discreet reminders where OS permits, and explicit taken/skipped user action with safe phone reconciliation. No independent clinical authority. Owner must approve concrete scope and device support before implementation. |
| 3 — Optional advanced watch support | Consented compatible health sensor summaries, better disconnected operation and interoperability | Only after actual hardware/platform feasibility, provenance, authorization, battery/reliability and privacy checks. Device-specific claims need measurement and permission evidence. |

**Design boundaries:** The phone remains the canonical medication-plan authority; watch actions must not independently alter clinician-verified schedules. Watch/phone divergence, duplicate taps, offline actions and permission denial need defined handling when Stage 2 is actually authorized. Watch apps are distinct from the post-MVP BLE/manufacturer medical-device initiative (IOT-001), though both may reuse approved provenance rules. No standalone watch authentication, cellular independence, sensor ingestion or guaranteed alarm timing is promised by this roadmap.

**Near-term action:** Preserve this roadmap entry only. Do not expand the current prototype handoff, start watch-specific ADRs/tests, or open watch development tasks until phone results justify Stage 2.
