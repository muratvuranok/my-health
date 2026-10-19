Status: ACCEPTED — single-product account/workspace architecture; professional feature release remains PROPOSED
Date: 2026-10-18
Owner: My Health MASTER
Owner acceptance: 2026-10-18 — explicit owner chat instruction that My Health is one product/account, with purpose selection at onboarding and profession-specific modules. Acceptance is structural only, not professional feature launch approval.
Supersedes: None
Superseded by: None

# ADR-017 — One My Health product with purpose-based onboarding and modular workspaces

## Owner decision
**One My Health product, one identity/account system, one common user data model and modular experiences.** During first account setup, the user selects what they intend to use My Health for. Supported future purpose options include:
- Personal health tracking
- Caregiving / family coordination
- Fitness coaching / training clients
- Nutrition counseling / dietetics
- Clinical patient follow-up (physician/qualified healthcare professional)

The chosen purpose determines which relevant onboarding steps, navigation modules and workspace entrypoints are made available, **subject to eligibility and separate authorization**. It does NOT create separate products, codebases, logins or independent health-data ownership systems. Selection is configurable later. One person can use personal tracking and operate one or more workspaces in different capacities. This is an **accepted structural design direction**, not approval to ship professional features, clinical activity or automated therapy changes.

## Key architectural distinctions
- **Account / identity:** authenticates a natural person. A single account can hold multiple contextual memberships; authentication never treats email domain or self-selected profession as proof of credentials.
- **Personal health profile:** individual's data is protected and individually controlled; not owned by coach/clinic/workspace.
- **Workspace:** optional management space for professional or organization use; has category, organization metadata, billing and staff membership (if later enabled). It is not a superuser account.
- **Workspace membership:** relationship between an authenticated user and a workspace, with role (owner/admin/practitioner/assistant). Workspace admin may configure modules and staff but does **not** automatically read clients' medical information.
- **Member/patient relationship + consent grant:** distinct, explicitly accepted authorization for professional access to a specific person's selected data and actions. Cross-account data sharing remains gated as a future phase consistent with ADR-001.
- **Role, profession and capability:** account-selected intended purpose affects UI configuration; professional credentials and regulated scopes require independent verification; backend enforces granted capabilities per actor, workspace, subject, purpose and data category.
- **Module catalog:** versioned registry of available capabilities, dependencies, eligibility, rollout states and feature flags; frontend renders allowed modules using **server-issued effective capabilities** checked against account/workspace policy and grants. Frontend visibility is never security authority.

## Candidate first-run user journey (UI copy, not final API)
1. User creates or signs into their existing My Health account using approved Identity methods.
2. Asks: **"How will you use My Health?"**. User chooses one or more purposes; personal health remains available across contexts.
3. For a professional purpose, gather minimal workspace name/type and show specific professional verification requirements where applicable. Do not collect clinical license data before necessity/privacy review.
4. Configure modules: a coach sees clients, check-in requests, training plans and progress; dietitian sees clients, measurement trends, nutrition plans; licensed clinician may later see authorized medication history and patient follow-up, subject to clinical/regulatory gate. Non-approved modules may be marked unavailable; never falsely present them as ready.
5. Patient/member separately accepts invitations and grants data categories, actions, timeframe, purpose and recipient. Before consent, a professional sees no health records.
6. Context switcher: **Personal** | **Coach workspace A** | **Dietitian workspace B** | **Clinic workspace C** where the authenticated account actually has membership/eligibility. Switching contexts changes navigation, not login or data ownership.
7. Roles and purposes are editable; remove or add workspace membership without deleting personal health data. Account deletion and workspace closure follow separately designed retention/erasure rules.

## Proposed module mapping (availability requires later owner GO)
| Purpose/context | Default UI concept | Sensitive data access |
|---|---|---|
| Personal | My timeline, medicines, measurements, reminders, integrations | Own authorized profile |
| Caregiver | Managed profiles, coordination, future family grants | Only lawfully authorized managed/granted subjects |
| Coach | Client roster, weekly check-ins, body measurements, workout plans | Explicit client grants, no medication by default |
| Dietitian | Client roster, diet plans, weight trends and follow-ups | Explicit client grants; medicine/allergy only if separately authorized and appropriate |
| Clinician | Patient roster, measurements, medication adherence review, follow-up | Verified clinical eligibility AND patient grant; clinical-order flow separately gated |
Workspace size 50+ clients is a nonfunctional target to validate with representative synthetic load, not a blanket limit or license to inspect 50 patient records.

## One product, two native clients
Per accepted ADR-005, the single My Health product/account experience is delivered through separate native iOS (Swift/SwiftUI) and Android (Kotlin/Jetpack Compose) apps. A unified product does not imply one cross-platform UI framework. No separate patient/coach/doctor apps are authorized.

## Single product vs service deployments
A single product/repository UX **does not require a monolith backend**. Accepted ADR-012 microservices+Kubernetes continues to govern implementation direction. Identity, Profile/Grants, Medication, Records, Notification and future Plans/Workspaces are logical bounded contexts with one public API namespace /v1 through BFF/gateway. Avoid creating one service/pod per persona. Prefer shared capabilities and permission-aware modules over duplicating medication and measurement data per workspace.

## Authorization model invariants
- All resources have canonical ownership, subject, provenance and audit; the same measurement may be granted for different purposes without duplicating or transferring ownership.
- Default deny; grants are explicit and revocable and have scoped `resource_categories`, `actions`, `history_window`, `workspace_id`, `professional_id`, `purpose`, `expires_at`, `grant_version`.
- Identity and workspace roles are insufficient by themselves to query protected profile data; each owning service validates effective permission and revocation epoch. No administrator bypass for clinical data.
- Separate clinical authorization and professional validation from self-reported "I am a doctor"; onboarding selection must never unlock patient data or prescription changes.
- License, professional registration, age/guardian rules, legal consent, country-specific practice rules, encryption key sharing and data residency are OPEN pending jurisdictional review.
- Coherent cancellation: when a person revokes consent or leaves a workspace, future reads, exports, async notifications and caches must respect it; locally downloaded data cannot be clawed back.
- No autonomous medication changes. Clinician-authored changes, if ever released, need independent regulated identity/proposal/approval workflow under ADR-003/009.

## Domain and API design implications — NOT FROZEN
Candidate entities: `account`, `onboarding_purpose`, `workspace`, `workspace_membership`, `professional_credentials`, `module_definition`, `workspace_module_configuration`, `subject_relationship`, `data_access_grant`, `audit_access_event`, `professional_plan`, `checkin_request`. Keep identity independent from UI purpose; role and permission are separate typed concepts.

Candidate endpoints (no implementation GO): `GET/PUT /v1/me/onboarding`, `GET /v1/me/contexts`, `POST/GET /v1/workspaces`, `GET /v1/workspaces/{id}/capabilities`, `POST /v1/workspaces/{id}/invitations`, `GET /v1/me/received-invitations`, `POST /v1/me/consent-grants`, `DELETE /v1/me/consent-grants/{id}`. Exact paths, request/response, role enums, scopes and error semantics remain DRAFT and require OpenAPI 3.1 review. Do not confuse UI module visibility with authorization.

## Essential negative and lifecycle tests
One account with personal+multiple professional contexts; changing purpose without losing profile; duplicate identity prevention; denied/unverified practitioner; coach reading medication without grant; clinic admin seeking unrelated medical data; two clinics with same patient but different consent; inviting 50+ clients with no access until acceptance; staff termination; consent revocation mid-request/export; multi-device stale token; context-switch auth leakage; account/workspace deletion and reconnection; marketing/payment flags not changing clinical authorization.

## Relationship to existing decisions
This **clarifies the single-product account architecture** while leaving ADR-001 initial personal scope, ADR-003 medication authority, ADR-012 deployment topology, ADR-015 messaging and ADR-016 integrations intact. The professional modules documented in FUTURE-001/002 remain **PROPOSED / POST-MVP** and are not implicitly approved for implementation. MVP scope, commercial plan and product/clinical launch gates remain OPEN.

## Evidence to accept professional extensions (NOT satisfied)
Obtain interview evidence showing at least one professional persona with willingness to pay, and demonstrate safe permissions via synthetic tests before incorporating any professional workspace entities, endpoints or tests into MVP. The owner-approved one-product direction alone is not evidence that the professional modules belong in MVP.

## Reversibility and evidence
Changing account/data ownership after production is high-cost; adopting a single identity with separate workspace membership is intentional. Module bundle and exact onboarding content remain reversible through capability flags. Validate personas with user research and real cross-context authorization tests before expanding the module catalog.
