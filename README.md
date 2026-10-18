# My Health
### Your Family Health Operating System — working product vision

**My Health** is an early-stage, privacy-first mobile health coordination product designed to help individuals and family caregivers organize medication routines, health measurements, and relevant health records in one understandable place.

**Status:** Planning / architecture review. No production application or clinical service is available yet. The name *My Health* is provisional.

## The problem
Managing several medicines, changing schedules, measurements and care responsibilities is fragmented across reminders, notes, documents and apps. Family caregivers often need to organize information for a parent or relative without losing context or control. A missed reminder, incorrect schedule or unclear record can have serious consequences.

## What we want to build
- **Medication routines:** Record medicines, plans, schedules, confirmations, skipped doses and historical changes, with safe handling of time zones and offline reminders.
- **Health timeline:** Keep selected measurements such as blood pressure, pulse and weight together with their units, timestamps and source.
- **Caregiver-friendly profiles:** Allow someone to organize information for themselves and, subject to permission and legal review, a person they care for. Cross-account sharing is a later, separately gated capability.
- **Clear exports:** Generate understandable summaries to discuss with healthcare professionals; not diagnosis or medical advice.
- **Device interoperability:** Explore Apple HealthKit and Android Health Connect, followed by other devices and integrations where technically and legally feasible.
- **Privacy by design:** Minimize sensitive data, make permissions understandable, and design for secure storage, export and deletion.

## Who it is for
Initial hypotheses: adults managing multiple medicines and regular measurements, and family caregivers coordinating a relative's routine. We will validate these needs with real user research; we do not claim product-market fit.

## Initial markets
**Türkiye and Azerbaijan first**, with **Turkish, Azerbaijani and English** language support planned. The architecture should accommodate future international markets, local time zones, measurement units, accessibility and jurisdiction-specific privacy requirements.

## Product principles
1. **Medication safety before convenience.** No silent schedule edits or automatic dosage recommendations.
2. **Offline reliability.** Core reminders and user workflows should not depend on an always-available server; mobile OS constraints require real-device verification.
3. **User control and consent.** Explicit provenance, ownership and permission boundaries.
4. **Interoperability without hidden overwrites.** Imported health data must not silently change authoritative medication plans.
5. **Accessible, multilingual design.** Suitable for older adults and caregivers as well as individual users.
6. **No sale of personal health data.** Do not claim clinical outcomes without evidence.

## Technology direction (not a frozen implementation contract)
| Area | Current direction |
| --- | --- |
| Mobile | React Native + TypeScript; iOS and Android, native Swift/Kotlin integrations where necessary |
| Backend | Go; small, clearly bounded microservices under architectural review |
| Data | PostgreSQL, service-owned data boundaries |
| API | Contract-first REST/JSON, OpenAPI 3.1, versioned request/response schemas |
| Infrastructure | Docker Compose for local development; Kubernetes/k3d for staged learning and deployment |
| Quality | Contract, unit, integration, security, offline sync, and real-device tests |

These are architectural directions, **not a claim that code, CI or production infrastructure exists**. Earlier SwiftUI/modular-monolith documents may be stale; see the decision log before implementing.

## Planned delivery
1. **Discovery and prototype:** User interviews and synthetic-data workflows, especially medication scheduling and caregiver usability.
2. **Safe mobile alpha:** Validate local reminders, offline records, accessibility, platform health integration and exports on real devices.
3. **Account and synchronization capabilities:** Introduce secure identity, backend services, multi-device sync and recovery only after contract, encryption and privacy decisions.
4. **Family coordination and broader integrations:** Permissioned sharing and compatible connected devices after validation and safety reviews.

Dates, prices and launch claims are hypotheses, not commitments.

## Project documentation
- [Product vision](docs/product/PRODUCT_VISION.md)
- [MVP scope — contains historical assumptions; under revision](docs/product/MVP_SCOPE.md)
- [Startup strategy](docs/business/STARTUP_STRATEGY.md)
- [Architecture audit and open risks](docs/architecture/ARCHITECTURE_AUDIT_V1.md)
- [Microservices + Kubernetes direction (ADR-012)](docs/decisions/ADR-012-MICROSERVICES-KUBERNETES.md)
- [API contract outline — NOT frozen](docs/backend/API_CONTRACT.md)
- [Authentication design — draft](docs/backend/AUTH_DESIGN.md)
- [MASTER governance — pending reconciliation](MASTER.md)
- [Security principles](SECURITY.md)

## What this project is not
It is **not** a medical device, diagnostic system, emergency service or source of medication instructions. It does not replace clinicians. No production medical data should be committed to this repository. The app is not yet released.

## Contributions and licensing
Architecture and API decisions are still being reviewed. No open-source license has been selected; a public repository does **not** automatically grant permission to reuse its code. Contribution policy and security reporting channel will be defined before accepting external contributions.

---
*My Health is a working name and early-stage product concept. Features and architecture are subject to research, technical validation and applicable laws.*
