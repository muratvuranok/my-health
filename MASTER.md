Status: ACCEPTED (governance; unresolved proposals remain OPEN)
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: MASTER v1.2
Superseded by: None

# MY HEALTH — MASTER v1.3

Repository: muratvuranok/my-health. **Temporarily PUBLIC** to permit external AI tools to read documentation; the owner intends to make it private after review. Public visibility is NOT permission to publish sensitive health data, secrets or private conversation content. Product brand remains a working name.

## Scope and authority
MASTER owns product strategy, accepted decisions, priorities, governance and release gates. Lili AI and Claude are implementation/review participants within explicit scoped tasks only. The My Health repository is not the Lili AI platform repository. Agent orchestration, agents and their lead are managed in a separate project; no agent infrastructure is part of this repository. No production deployment, live health records, secrets, autonomous clinical dosing advice or public health claims without explicit MASTER GO.

## Document authority order
1. **MASTER** — owner-approved governance and decision status.
2. **ACCEPTED ADR / STR** — binding decisions in their domains; owner-approved later amendments can supersede earlier decisions.
3. **ARCHITECTURE_AUDIT** — risk register and contract-freeze hold, not authority to reverse ACCEPTED decisions.
4. **Plans/specifications** — draft design detail, binding only once accepted.
5. **Handoff** — implementation instructions only within a current explicit authorization.
**FUTURE/PROPOSED module docs must not add entities, endpoints or tests to MVP scope until owner GO after interview evidence.** A document may list candidates for future phases only; they are non-binding for MVP implementation. **SUPERSEDED** and **SUSPENDED** documents are historical and NOT executable authority. **PROPOSED** and **DRAFT/OPEN** items cannot reverse accepted decisions. Conflict resolution: stop the affected implementation, record the ambiguity, seek an owner decision; do not infer acceptance from recency or an agent's recommendation.

## Accepted decisions — unchanged
- **STR-001 ACCEPTED:** Türkiye and Azerbaijan first, then validated global expansion.
- **ADR-001 ACCEPTED:** individual-first, with owner-approved 2026-10-19 same-device caregiver-managed profile in MVP; cross-account sharing is still deferred.
- **ADR-002 ACCEPTED:** encrypted local data plus controlled cloud sync as architectural intent; encryption/key model OPEN.
- **ADR-003 ACCEPTED:** My Health authoritative verified medication schedule; imports cannot silently overwrite it.
- **ADR-004 ACCEPTED:** private TestFlight before public release, gated for real data.
- **ADR-007 ACCEPTED (conventions only):** contract-first OpenAPI 3.1, snake_case, errors/compatibility; endpoint schemas and contract freeze remain OPEN.
- **ADR-017 ACCEPTED (reaffirmed by owner 2026-10-19; single-product account/workspace structure):** One My Health identity and application; user chooses intended purpose at onboarding; module visibility and workspace contexts vary by eligibility, but workspace admin roles never confer patient-data access. Professional modules remain PROPOSED and gated; acceptance of a shared account does not add workspace schema to MVP.
- **ADR-012 ACCEPTED IN PRINCIPLE:** bounded microservices and Kubernetes learning-first target. **ADR-013 NOT SELECTED**; no reversion to monolith approved.
- **IOT-001 / MH-IOT-01:** one unified post-MVP connected-device initiative (tracking alias, not a second decision).

- **ADR-005 ACCEPTED (revised later 2026-10-19):** Owner withdrew the earlier same-day React Native/Expo selection (main commit 4d9d20b). Current mobile direction is **native iOS Swift/SwiftUI plus Android Kotlin/Jetpack Compose**. **MH-PROT-001 prototype platforms: iOS and Android in parallel (owner decision 2026-10-19).** Public release order and real-device validation remain OPEN.
- **ADR-014 ACCEPTED (initial alpha only):** Owner selected local-only initial alpha 2026-10-19. ADR-002 future cloud-sync intent remains intact; ADR-010 OPEN.
## Open decisions / proposals
- **ADR-015 PROPOSED (no owner acceptance recorded):** device-local reminders, optional push and transactional email with privacy-first asynchronous delivery; notification API draft NOT FROZEN.
- **ADR-016 PROPOSED (no owner acceptance recorded):** per-platform adapters, minimal per-type data access, separated OS/user/server authorization and provenance; integration API draft NOT FROZEN.
- **ADR-013 NOT SELECTED:** Owner reaffirmed ADR-012 microservices + Kubernetes on 2026-10-19. ADR-013 is historical alternative.
- **ADR-009 DRAFT/OPEN:** medication temporal domain including PRN, DST, travel, tapering, history.
- **ADR-010 DRAFT/OPEN:** server-readable vs E2EE vs local-only alpha option, sync/conflict protocol and recovery; ADR-002 remains accepted.
- **ADR-019 PROPOSED:** permissioned in-app messages between clients and dietitians, coaches, physicians or authorized caregivers. This is separate from accepted ADR-015 outbound email/push delivery. Encryption/clinical record retention remain OPEN.
- **ADR-018 PROPOSED:** optional purchasable modules and subscription entitlement models; core medication safety not paywalled, commercial pricing and provider OPEN. One account and modular contexts remain accepted under ADR-017.
- **ADR-006/008/011 NOT YET WRITTEN:** tech stack versions, auth state machine, consent/grants/retention. ADR-007 conventions are accepted but field-level OpenAPI is NOT approved. No implicit choice.
- Hosting region and Türkiye/Azerbaijan legal/KVKK transfer review OPEN; pricing and business model OPEN; trademark/brand clearance OPEN; activation metric OPEN; web timing OPEN; actual Health Connect/HealthKit OS matrix OPEN.

## Implementation status
**ARCHITECTURE STATUS:** ADR-005 mobile, ADR-007 API conventions, ADR-012 microservices/Kubernetes, ADR-014 local-only initial alpha and ADR-017 single-product structure are accepted. The same-device caregiver-managed MVP profile is explicitly approved. ADR-015 notifications and ADR-016 integrations remain PROPOSED pending explicit owner approval; their API drafts are not frozen. **CONTRACT FREEZE: NO-GO** — existing API_CONTRACT and AUTH_DESIGN are outlines, not final OpenAPI request/response schemas. **CLAUDE_BACKEND_EXECUTION: SUSPENDED**; old local implementation GO is invalid for current topology and contract risk. A new implementation authorization requires an approved architecture/contract scope and current task. The owner selected **parallel native iOS + Android** for MH-PROT-001 (2026-10-19). The handoff remains **PLAN ONLY**; implementation requires a separate scoped GO. This selects prototype platforms only, not a parallel-agent system in this repository. No code or CI is implied by these documents. Synthetic prototypes may be separately authorized; no claim of passed tests or legal compliance.

## Release gates
GATE-00 privacy/scope/security OPEN; GATE-01 medication correctness OPEN; GATE-02 Apple/Android platform feasibility OPEN; GATE-03 sync/encryption/restore OPEN; GATE-04 real-data beta/TestFlight BLOCKED until prerequisites and explicit GO.

## Document index (tracked Markdown)
| File | Status / reading notes |
|---|---|
| [`MASTER.md`](MASTER.md) | ACCEPTED v1.3 — authoritative |
| [`README.md`](README.md) | DRAFT — agent navigation only |
| [`ROADMAP.md`](ROADMAP.md) | DRAFT, v2 PROPOSED |
| [`SECURITY.md`](SECURITY.md) | DRAFT — protection requirements |
| [`docs/product/PRODUCT_VISION.md`](docs/product/PRODUCT_VISION.md) | DRAFT vision |
| [`docs/product/MVP_SCOPE.md`](docs/product/MVP_SCOPE.md) | DRAFT under revision |
| [`docs/product/FUTURE-001-COACH-WORKSPACE.md`](docs/product/FUTURE-001-COACH-WORKSPACE.md) | PROPOSED / BACKLOG |
| [`docs/product/FUTURE-003-NUTRITION-MEAL-PLANS.md`](docs/product/FUTURE-003-NUTRITION-MEAL-PLANS.md) | PROPOSED — dietitian plans and client meal tracking |
| [`docs/product/FUTURE-002-CARE-TEAM-WORKSPACES.md`](docs/product/FUTURE-002-CARE-TEAM-WORKSPACES.md) | PROPOSED / BACKLOG; clinical use gated |
| [`docs/future/FUTURE-004-WATCH.md`](docs/future/FUTURE-004-WATCH.md) | BACKLOG — not scheduled; watch companions |
| [`docs/future/FUTURE-005-ACTIVITY.md`](docs/future/FUTURE-005-ACTIVITY.md) | BACKLOG — not scheduled; workouts/routes |
| [`docs/future/FUTURE-006-SLEEP.md`](docs/future/FUTURE-006-SLEEP.md) | BACKLOG — not scheduled; S3 audio reconsideration pending |
| [`docs/product/OVERVIEW.md`](docs/product/OVERVIEW.md) | DRAFT product overview |
| [`docs/business/STARTUP_STRATEGY.md`](docs/business/STARTUP_STRATEGY.md) | DRAFT hypotheses |
| [`docs/architecture/SYSTEM_ARCHITECTURE.md`](docs/architecture/SYSTEM_ARCHITECTURE.md) | SUPERSEDED topology; historical |
| [`docs/architecture/ARCHITECTURE_AUDIT_V1.md`](docs/architecture/ARCHITECTURE_AUDIT_V1.md) | DRAFT review; contract-freeze hold |
| [`docs/decisions/ADR-001-004.md`](docs/decisions/ADR-001-004.md) | ACCEPTED owner decisions |
| [`docs/decisions/STR-001-MARKETS.md`](docs/decisions/STR-001-MARKETS.md) | ACCEPTED market decision |
| [`docs/decisions/ADR-005-MOBILE-PLATFORM.md`](docs/decisions/ADR-005-MOBILE-PLATFORM.md) | ACCEPTED native Swift/SwiftUI + Kotlin/Compose; earlier RN choice superseded |
| [`docs/decisions/ADR-007-API-CONVENTIONS.md`](docs/decisions/ADR-007-API-CONVENTIONS.md) | ACCEPTED conventions; payloads not frozen |
| [`docs/decisions/ADR-009-MEDICATION-DOMAIN.md`](docs/decisions/ADR-009-MEDICATION-DOMAIN.md) | DRAFT / OPEN |
| [`docs/decisions/ADR-010-SYNC-ENCRYPTION.md`](docs/decisions/ADR-010-SYNC-ENCRYPTION.md) | DRAFT / OPEN |
| [`docs/decisions/ADR-012-MICROSERVICES-KUBERNETES.md`](docs/decisions/ADR-012-MICROSERVICES-KUBERNETES.md) | ACCEPTED principle; details OPEN |
| [`docs/decisions/ADR-015-NOTIFICATIONS-MESSAGING.md`](docs/decisions/ADR-015-NOTIFICATIONS-MESSAGING.md) | PROPOSED notification architecture; endpoint schema OPEN |
| [`docs/decisions/ADR-016-INTEGRATIONS-PERMISSIONS.md`](docs/decisions/ADR-016-INTEGRATIONS-PERMISSIONS.md) | PROPOSED integration boundaries; permission implementation OPEN |
| [`docs/decisions/ADR-017-UNIFIED-PRODUCT-MODULAR-WORKSPACES.md`](docs/decisions/ADR-017-UNIFIED-PRODUCT-MODULAR-WORKSPACES.md) | ACCEPTED single product, modular workspaces; professional releases PROPOSED |
| [`docs/decisions/ADR-019-PERMISSIONED-MESSAGING.md`](docs/decisions/ADR-019-PERMISSIONED-MESSAGING.md) | PROPOSED messaging and relationship/consent design |
| [`docs/decisions/ADR-018-MODULAR-ENTITLEMENTS-MONETIZATION.md`](docs/decisions/ADR-018-MODULAR-ENTITLEMENTS-MONETIZATION.md) | PROPOSED commercial packaging and entitlements |
| [`docs/decisions/ADR-013-PRODUCT-FIRST-DEPLOYMENT.md`](docs/decisions/ADR-013-PRODUCT-FIRST-DEPLOYMENT.md) | NOT SELECTED; historical alternative |
| [`docs/decisions/ADR-014-LOCAL-ONLY-ALPHA.md`](docs/decisions/ADR-014-LOCAL-ONLY-ALPHA.md) | ACCEPTED (local-first alpha only) |
| [`docs/backend/BACKEND_MASTER_PLAN_V1.md`](docs/backend/BACKEND_MASTER_PLAN_V1.md) | SUPERSEDED and ON HOLD |
| [`docs/backend/API_CONTRACT.md`](docs/backend/API_CONTRACT.md) | DRAFT / NOT FROZEN |
| [`docs/backend/NOTIFICATION_API_DRAFT.md`](docs/backend/NOTIFICATION_API_DRAFT.md) | DRAFT notification request/response |
| [`docs/backend/INTEGRATION_API_DRAFT.md`](docs/backend/INTEGRATION_API_DRAFT.md) | DRAFT integration request/response |
| [`docs/backend/AUTH_DESIGN.md`](docs/backend/AUTH_DESIGN.md) | DRAFT / unresolved auth options |
| [`docs/backend/LOCAL_ACCEPTANCE.md`](docs/backend/LOCAL_ACCEPTANCE.md) | DRAFT historic planned acceptance |
| [`docs/backend/DATA_MODEL.md`](docs/backend/DATA_MODEL.md) | DRAFT TODO |
| [`docs/security/THREAT_MODEL.md`](docs/security/THREAT_MODEL.md) | DRAFT TODO |
| [`docs/integrations/CONNECTED_DEVICES.md`](docs/integrations/CONNECTED_DEVICES.md) | DRAFT (IOT-001/MH-IOT-01) |
| [`docs/testing/RELEASE_GATES.md`](docs/testing/RELEASE_GATES.md) | DRAFT gates OPEN |
| [`docs/reviews/REVIEW-2026-10-18.md`](docs/reviews/REVIEW-2026-10-18.md) | DRAFT review record |
| [`handoff/MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md`](handoff/MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md) | PLAN ONLY; parallel native iOS + Android decided 2026-10-19; implementation GO separate |
| [`handoff/CLAUDE_BACKEND_EXECUTION.md`](handoff/CLAUDE_BACKEND_EXECUTION.md) | SUSPENDED — NEVER EXECUTE |
| [`handoff/LILI_AI_IMPLEMENTATION_CONTRACT.md`](handoff/LILI_AI_IMPLEMENTATION_CONTRACT.md) | DRAFT — must follow current task |
| [`archive/conversation-index.md`](archive/conversation-index.md) | DRAFT metadata index; not verbatim transcript |
| [`archive/decision-history.md`](archive/decision-history.md) | DRAFT history; not authority |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | DRAFT workflow template |
| [`.github/CODEOWNERS`](.github/CODEOWNERS) | Owner mapping |
| [`.gitignore`](.gitignore) | Safety hygiene configuration |

## Tasks and change control
[TASKS.md](TASKS.md) is the single task status index, with GitHub Issues as delivery evidence. Never treat a historic handoff as an active task. Each ADR must document alternative, reversal cost and evidence that changes the recommendation. No production or live health-data action until gates and owner GO. Review evidence and PR required before acceptance.
