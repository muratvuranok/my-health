Status: BACKLOG — NOT SCHEDULED; NO IMPLEMENTATION GO
Date: 2026-10-19
Owner: My Health MASTER

# FUTURE-004 — Smartwatch apps

**Archived concept, not implementation authority.** This is supporting context only; MH-PROT-001 is unchanged. Future specification requires explicit owner phase GO and evidence of user value.

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

## Device integration notes
## Smartwatch product track (separate from medical-device BLE integration)
As of the owner's phased roadmap direction on 2026-10-19, Apple Watch/watchOS and Android Wear OS **companion applications** are planned for a post-MVP phase. Their first possible scope is daily medication overview, privacy-preserving reminders and user-confirmed taken/skipped actions synchronized safely with the phone. This is **not part of MH-PROT-001** and does not approve any watch implementation.

A watch companion app is **not the same integration** as a Bluetooth blood-pressure cuff, scale or custom ESP32 medical prototype. Health-sensor ingestion or standalone/disconnected watch use remains a later proposal requiring model/platform-specific feasibility and privacy evidence. See [ROADMAP.md](../../ROADMAP.md) for stages; avoid creating a separate execution stream before the phone foundation is usable.
