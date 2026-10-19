Status: DRAFT — single task queue / no active implementation GO
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# My Health — TASKS

This is the single task-priority index; GitHub Issues/PRs contain details and evidence. No older handoff activates work on its own.

| Task | Status | Reference | Gate |
|---|---|---|---|
| MH-REV-001 Reconcile planning documents | IN REVIEW | 2026-10-18 architecture review PR | Docs only |
| MH-ADR-019 In-app client-professional messaging | PROPOSED / BACKLOG | ADR-019 | Authorization, encryption, retention, privacy and clinical review OPEN |
| MH-ADR-018 Optional paid modules, bundles and entitlements | PROPOSED / COMMERCIAL MODEL OPEN | ADR-018 | Store policy, pricing, downgrade and privacy review |
| MH-ADR-017 Single product and purpose-based onboarding | ACCEPTED STRUCTURE / IMPLEMENTATION NOT AUTHORIZED | ADR-017 | Module release, consent and professional verification remain OPEN |
| MH-FUTURE-003 Dietitian-created weekly meal plan and member logging | PROPOSED / BACKLOG | docs/product/FUTURE-003-NUTRITION-MEAL-PLANS.md | Consent, versioned API and legal review before development |
| MH-FUTURE-002 Patient-controlled care team (dietitian/trainer/physician) | PROPOSED / BACKLOG | docs/product/FUTURE-002-CARE-TEAM-WORKSPACES.md | Medical/legal review and explicit owner GO |
| MH-FUTURE-001 Coach workspace and consented member tracking | PROPOSED / BACKLOG | docs/product/FUTURE-001-COACH-WORKSPACE.md | Separate owner GO and privacy/authorization ADR required |
| MH-ADR-005 Cross-platform decision | PROPOSED | ADR-005 | Owner acceptance |
| MH-ADR-016 Integration permission matrix and typed contracts | ARCHITECTURE ACCEPTED / API DRAFT | ADR-016, INTEGRATION_API_DRAFT.md | Real device and privacy validation OPEN |
| MH-ADR-015 Notification, email and push | ARCHITECTURE ACCEPTED / API DRAFT | ADR-015, NOTIFICATION_API_DRAFT.md | Provider selection and real delivery OPEN |
| MH-ADR-007 API conventions | CONVENTIONS ACCEPTED / SCHEMAS OPEN | ADR-007 | OpenAPI contract freeze NOT granted |
| MH-ADR-009 Medication time/domain rules | OPEN | ADR-009 | Owner/domain review |
| MH-ADR-010 Sync and encryption | OPEN | ADR-010 | Owner/security/legal review |
| MH-ADR-013 Monolith-first amendment | PROPOSED | ADR-013 | Does NOT override ADR-012 |
| MH-ADR-014 Local-only alpha | PROPOSED | ADR-014 | Does NOT override ADR-002 |
| MH-BE-001 Local backend implementation | SUSPENDED | CLAUDE_BACKEND_EXECUTION.md; Issue #19 | New architecture/contract GO required |
| MH-BE-002 Auth implementation | BLOCKED pending contract | AUTH_DESIGN.md; Issue #20 | No executable authorization |
| MH-001 / MH-002 mobile prototype | PLANNING ONLY | LILI_AI_IMPLEMENTATION_CONTRACT.md | New explicit scoped GO required |

No assumptions about closed/issues outside this index; review live GitHub issue status before action.
