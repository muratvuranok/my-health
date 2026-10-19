Status: DRAFT — single task queue / no active implementation GO
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# My Health — TASKS

This is the single task-priority index; GitHub Issues/PRs contain details and evidence. No older handoff activates work on its own.

## OWNER DECISION REQUIRED — before any new prototype task

**No new prototype or implementation task may begin until these owner decisions are explicitly recorded.** These are choices to be made, not implicit approvals, and they do not authorize use of real health data.

1. **ADR-005 mobile platform:** ACCEPT / REJECT the React Native + TypeScript cross-platform proposal.
2. **ADR-013 modular-monolith-first deployment amendment:** ACCEPT / REJECT. Accepted ADR-012 microservices/Kubernetes remains effective until an explicit change.
3. **ADR-014 local-only alpha:** ACCEPT / REJECT. Accepted ADR-002 cloud-sync architectural intent remains unchanged unless amended.
4. **E3 caregiver-managed profile in MVP:** YES / NO; reconcile with ADR-001 individual-first and separately controlled future grants.
5. **ADR-017 single-product structure acceptance:** CONFIRM / REVISE the documented owner-chat acceptance scope. This does not authorize professional schemas or modules without interviews.
6. **Repository visibility:** Is external review finished? YES / NO; if YES, owner should return the temporarily public repository to private via GitHub settings.

Additional pending owner approvals: **ADR-015 notification architecture**, **ADR-016 integration architecture**, field-level OpenAPI contract, encryption/hosting, clinician permissions and commercial pricing. Do not interpret merger of a documentation PR as acceptance of these proposals.

| Task | Status | Reference | Gate |
|---|---|---|---|
| MH-REV-001 Reconcile planning documents | IN REVIEW | 2026-10-18 architecture review PR | Docs only |
| MH-ADR-019 In-app client-professional messaging | PROPOSED / BACKLOG | ADR-019 | Authorization, encryption, retention, privacy and clinical review OPEN |
| MH-ADR-018 Optional paid modules, bundles and entitlements | PROPOSED / COMMERCIAL MODEL OPEN | ADR-018 | Store policy, pricing, downgrade and privacy review |
| MH-ADR-017 Single product and purpose-based onboarding | STRUCTURE ACCEPTED (owner chat) / IMPLEMENTATION NOT AUTHORIZED | ADR-017 | Module release, consent and professional verification remain OPEN |
| MH-FUTURE-003 Dietitian-created weekly meal plan and member logging | PROPOSED / BACKLOG | docs/product/FUTURE-003-NUTRITION-MEAL-PLANS.md | Consent, versioned API and legal review before development |
| MH-FUTURE-002 Patient-controlled care team (dietitian/trainer/physician) | PROPOSED / BACKLOG | docs/product/FUTURE-002-CARE-TEAM-WORKSPACES.md | Medical/legal review and explicit owner GO |
| MH-FUTURE-001 Coach workspace and consented member tracking | PROPOSED / BACKLOG | docs/product/FUTURE-001-COACH-WORKSPACE.md | Separate owner GO and privacy/authorization ADR required |
| MH-ADR-005 Cross-platform decision | PROPOSED | ADR-005 | Owner acceptance |
| MH-ADR-016 Integration permission matrix and typed contracts | PROPOSED ARCHITECTURE / API DRAFT | ADR-016, INTEGRATION_API_DRAFT.md | Real device and privacy validation OPEN |
| MH-ADR-015 Notification, email and push | ARCHITECTURE ACCEPTED / API DRAFT | ADR-015, NOTIFICATION_API_DRAFT.md | Provider selection and real delivery OPEN |
| MH-ADR-007 API conventions | CONVENTIONS ACCEPTED (owner chat) / SCHEMAS OPEN | ADR-007 | OpenAPI contract freeze NOT granted |
| MH-ADR-009 Medication time/domain rules | OPEN | ADR-009 | Owner/domain review |
| MH-ADR-010 Sync and encryption | OPEN | ADR-010 | Owner/security/legal review |
| MH-ADR-013 Monolith-first amendment | PROPOSED | ADR-013 | Does NOT override ADR-012 |
| MH-ADR-014 Local-only alpha | PROPOSED | ADR-014 | Does NOT override ADR-002 |
| MH-BE-001 Local backend implementation | SUSPENDED | CLAUDE_BACKEND_EXECUTION.md; Issue #19 | New architecture/contract GO required |
| MH-BE-002 Auth implementation | BLOCKED pending contract | AUTH_DESIGN.md; Issue #20 | No executable authorization |
| MH-001 / MH-002 mobile prototype | PLANNING ONLY | LILI_AI_IMPLEMENTATION_CONTRACT.md | New explicit scoped GO required |

No assumptions about closed/issues outside this index; review live GitHub issue status before action.
