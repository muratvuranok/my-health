Status: DRAFT — single task queue / no active implementation GO
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# My Health — TASKS

This is the single task-priority index; GitHub Issues/PRs contain details and evidence. No older handoff activates work on its own. User decisions are recorded below; **only implementation/release gates remain blocking**.

## OWNER DECISIONS RECORDED — 2026-10-19

1. ADR-005 mobile: **REVISED LATER ON 2026-10-19 — ACCEPTED NATIVE iOS Swift/SwiftUI + Android Kotlin/Jetpack Compose**. Earlier same-day React Native + Expo choice (4d9d20b) is withdrawn.
2. ADR-013 monolith-first: **REJECT / NOT SELECTED** — ADR-012 Go microservices + Kubernetes retains authority.
3. ADR-014 initial alpha: **ACCEPT** — local-only first; long-term sync direction ADR-002 unchanged.
4. E3 caregiver-managed profile in MVP: **YES**, on the same device; no cross-account grants.
5. ADR-017 one app/account/onboarding structure: **ACCEPTANCE REAFFIRMED**; professional modules remain BACKLOG.
6. Repository visibility: **OPEN** — no instruction to make private; external review may continue. Visibility change requires owner choice.
7. First prototype platform: **DECIDED — iOS and Android in parallel**, native Swift/SwiftUI and Kotlin/Jetpack Compose (owner decision 2026-10-19). This resolves only the platform-selection gate; MH-PROT-001 still requires separate scoped implementation GO.

**ADR-010 remains OPEN** for cloud encryption, key custody, sync and hosting. ADR-009 medication temporal model, field-level OpenAPI, real-device integration/reminder/security proofs, and real-data release gates remain OPEN. ADR-015/016 retain PROPOSED status.

**MH-PROT-001:** [Synthetic mobile prototype handoff](handoff/MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md) is PREPARED. A separate scoped implementation GO is required before writing the app. Suspended legacy backend tasks remain blocked.

| Task | Status | Reference | Gate |
|---|---|---|---|
| MH-REV-001 Reconcile planning documents | DONE (PR #21, squash 46e92bb; R8 PR #22) | 2026-10-18 architecture review PR | Docs only |
| MH-PROT-001 Synthetic native mobile prototype | PLAN ONLY — both platforms selected; separate implementation GO required | handoff/MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md | iOS + Android parallel; shared deterministic JSON fixtures and platform-specific tests; synthetic-only/offline; no provider/backend |
| MH-SLEEP-01 Sleep history and optional analysis | BACKLOG — not scheduled | docs/future/FUTURE-006-SLEEP.md | S3 microphone recording OWNER DECISION PENDING — consider removal; no implementation GO |
| MH-ACTIVITY-01 Workouts and GPS route maps | BACKLOG — not scheduled | docs/future/FUTURE-005-ACTIVITY.md | Separate future owner GO required |
| MH-WATCH-01 Apple Watch and Wear OS companions | BACKLOG — not scheduled | docs/future/FUTURE-004-WATCH.md | Separate future owner GO required |
| MH-ADR-019 In-app client-professional messaging | PROPOSED / BACKLOG | ADR-019 | Authorization, encryption, retention, privacy and clinical review OPEN |
| MH-ADR-018 Optional paid modules, bundles and entitlements | PROPOSED / COMMERCIAL MODEL OPEN | ADR-018 | Store policy, pricing, downgrade and privacy review |
| MH-ADR-017 Single product and purpose-based onboarding | ACCEPTED / PROFESSIONAL MODULES DEFERRED | ADR-017 | Module release, consent and professional verification remain OPEN |
| MH-FUTURE-003 Dietitian-created weekly meal plan and member logging | PROPOSED / BACKLOG | docs/product/FUTURE-003-NUTRITION-MEAL-PLANS.md | Consent, versioned API and legal review before development |
| MH-FUTURE-002 Patient-controlled care team (dietitian/trainer/physician) | PROPOSED / BACKLOG | docs/product/FUTURE-002-CARE-TEAM-WORKSPACES.md | Medical/legal review and explicit owner GO |
| MH-FUTURE-001 Coach workspace and consented member tracking | PROPOSED / BACKLOG | docs/product/FUTURE-001-COACH-WORKSPACE.md | Separate owner GO and privacy/authorization ADR required |
| MH-ADR-005 Native iOS/Android mobile platform | ACCEPTED (revised 2026-10-19) | ADR-005 | Parallel iOS+Android prototype selected; release order and real-device safety matrix OPEN |
| MH-ADR-016 Integration permission matrix and typed contracts | PROPOSED ARCHITECTURE / API DRAFT | ADR-016, INTEGRATION_API_DRAFT.md | Real device and privacy validation OPEN |
| MH-ADR-015 Notification, email and push | PROPOSED / API DRAFT | ADR-015, NOTIFICATION_API_DRAFT.md | Provider selection and real delivery OPEN |
| MH-ADR-007 API conventions | CONVENTIONS ACCEPTED (owner chat) / SCHEMAS OPEN | ADR-007 | OpenAPI contract freeze NOT granted |
| MH-ADR-009 Medication time/domain rules | OPEN | ADR-009 | Owner/domain review |
| MH-ADR-010 Sync and encryption | OPEN | ADR-010 | Owner/security/legal review |
| MH-ADR-013 Monolith-first amendment | NOT SELECTED | ADR-013 | Does NOT override ADR-012 |
| MH-ADR-014 Local-only alpha | ACCEPTED (initial alpha only) | ADR-014 | Does NOT override ADR-002 |
| MH-BE-001 Local backend implementation | SUSPENDED | CLAUDE_BACKEND_EXECUTION.md; Issue #19 | New architecture/contract GO required |
| MH-BE-002 Auth implementation | BLOCKED pending contract | AUTH_DESIGN.md; Issue #20 | No executable authorization |
| MH-001 / MH-002 mobile prototype | PLANNING ONLY | LILI_AI_IMPLEMENTATION_CONTRACT.md | New explicit scoped GO required |

No assumptions about closed/issues outside this index; review live GitHub issue status before action.
