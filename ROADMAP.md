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
4. Historical proposal: k3d modular-monolith-first evaluation (ADR-013 NOT SELECTED; ADR-012 remains accepted); not an active task.
5. Month 4+: revisit hosting, encryption, API/sync and family beta with actual evidence.
Android market-share claims for TR/AZ are **to be verified** with dated evidence.

## Scope policy — delivery first (2026-10-19)
- **First deliver MH-PROT-001**: parallel native iOS + Android phone prototypes, scoped separately by explicit implementation GO. No watch, route, sleep, audio, new backend, or agent orchestration work may enter this handoff implicitly.
- New post-MVP ideas are recorded **once in TASKS.md** as a single backlog row (ID, short title, **BACKLOG — not scheduled**). Do not add competitor analysis, UX flows, proposed tests, APIs or architecture to active ROADMAP merely because an idea was mentioned.
- A future item gets its own `docs/future/` detail **only when the owner opens that stage**; existing pre-policy detail is preserved below in three archival FUTURE files, not as active work.
- Future items need user-value evidence, prioritization and their own implementation GO before work starts. Retain privacy caveats without turning them into current-project blockers.

## Unscheduled future candidates
- **MH-WATCH-01:** Apple Watch/watchOS and Wear OS companions — [FUTURE-004](docs/future/FUTURE-004-WATCH.md); **BACKLOG — not scheduled**.
- **MH-ACTIVITY-01:** Workout history, walking and GPS route display — [FUTURE-005](docs/future/FUTURE-005-ACTIVITY.md); **BACKLOG — not scheduled**.
- **MH-SLEEP-01:** Sleep history and optional later analyses — [FUTURE-006](docs/future/FUTURE-006-SLEEP.md); **BACKLOG — not scheduled**. **S3 microphone recording: OWNER DECISION PENDING — consider removal**.
