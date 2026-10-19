Status: DRAFT — public-safe overview; features not committed
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# My Health — Your Family Health Operating System (working product vision)

My Health is an early-stage privacy-first mobile health coordination product for people who organize their own or a relative's medication routines, selected health measurements, and relevant health records in one understandable place. **It is a planning-stage concept, not a medical device or released service.** Name is provisional.

## The problem
Several medications, changing schedules, measurements and caregiving responsibilities are commonly scattered across apps, handwritten notes and documents. Users need a trustworthy timeline and clear reminders without losing context or control.

## Planned capabilities
- Medication plans, reminders, logged actions, skipped doses and auditable plan history; safe timezone and offline handling.
- Health measurements with units, times, source and provenance; optional HealthKit / Health Connect permissions.
- Caregiver-friendly managed profiles and, after authorization/security review, future account-to-account sharing.
- Accessible daily timeline and patient-controlled export for clinician discussion, not diagnosis.
- Privacy-first local security, explicit consent and controlled cross-device recovery/sync once architecture is approved.
- Future device interoperability without silent schedule modification or clinical dose recommendations.

## People, markets and language
Initial hypotheses: adults managing multiple medicines and regular measurements, and family caregivers. Türkiye and Azerbaijan first; Turkish, Azerbaijani and English localization; global expansion after validation. Conduct 30–40 interviews and synthetic prototype studies to discover what users actually need.

## Product principles
Medication safety; reliable offline behavior within mobile OS constraints; provenance and user control; accessibility and multilingual UX; no medical diagnosis or automatic dosage advice; no sale of personal health information; no invented outcomes or traction.

## Current technical direction, not frozen
**ACCEPTED ADR-005:** native Swift/SwiftUI iOS and Kotlin/Jetpack Compose Android mobile clients. MH-PROT-001 prototypes will be planned for iOS and Android in parallel (owner decision 2026-10-19); public release order remains OPEN. Go microservices + Kubernetes learning-first is **ACCEPTED ADR-012 in principle**. PostgreSQL and OpenAPI 3.1 are the target design; auth, sync encryption, hosting and exact request/response semantics remain OPEN. No deployed app, running CI or production backend is claimed.

## Roadmap hypotheses
1. Research and synthetic clickable mobile prototype.
2. Real-device reminder/health-adapter capability and privacy validation.
3. Account, encrypted sync and family permissions when safety/contract gates pass.
4. Later broader device integrations, global expansion and commercial experiments.

Pricing, business model and brand are hypotheses, not approved commitments. The repository's definitive source for authority and open decisions is [MASTER.md](../../MASTER.md); see [PRODUCT_VISION.md](PRODUCT_VISION.md), [MVP_SCOPE.md](MVP_SCOPE.md) and [STARTUP_STRATEGY.md](../business/STARTUP_STRATEGY.md).
