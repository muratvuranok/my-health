Status: PROPOSED — future product expansion only; NOT an implementation/release authorization
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

> **BACKLOG — does not shape MVP data model, endpoints, tests, deployments or release scope.** This is a post-MVP hypothesis only. Interview evidence and explicit owner GO are mandatory before promotion. Profession-specific features cannot silently alter accepted ADR-001 personal-first scope.

# FUTURE-002 — Patient-controlled care team and professional workspaces

## Accepted product integration direction (ADR-017)
This is **one My Health application and identity system**, not separate patient/coach/dietitian/doctor products. Purpose-based onboarding can enable a workspace/context and relevant UI modules once each module is approved; users can switch between personal and professional contexts under the same account. Workspace owner/admin is not an unrestricted health-data superuser. Professional features in this document remain PROPOSED despite the accepted single-product structure. See [ADR-017](../decisions/ADR-017-UNIFIED-PRODUCT-MODULAR-WORKSPACES.md).

## Vision and relationship to FUTURE-001
My Health may connect an individual's personal health timeline to distinct approved professional relationships: physician, registered dietitian/nutrition professional, trainer/coach and eventually caregiver. A patient could invite a verified professional or accept an invitation, authorize exact data categories and duration, and revoke access later. Professionals may have caseloads of 50+ clients and organization/team workspaces. This extends [FUTURE-001 Coach Workspace](FUTURE-001-COACH-WORKSPACE.md); it does NOT approve real clinical workflows, prescribing, automated dosing, or unrestricted professional access.

## Nutrition module design detail
The proposed dietitian-created weekly menu and client meal tracking experience is described in [FUTURE-003](FUTURE-003-NUTRITION-MEAL-PLANS.md), including day/meal slots, portions, alternatives, adherence logging, future-effective versions and permission checks. It remains PROPOSED and separate from medication schedule authority.

## Example scenarios — hypotheses, not MVP commitments
1. Dietitian and patient: patient selectively shares weight trend, body measurements, food-related check-ins and **optionally** medication list or allergies where clinically relevant and lawful. Dietitian drafts a weekly nutrition plan for patient review and acceptance; does not infer medical suitability from unverified medication lists.
2. Trainer and member: person shares weight and agreed activity/measurement data; trainer drafts exercise sessions and check-in requests. Medication list is hidden unless independently and explicitly granted and demonstrably necessary.
3. Physician and patient: patient provides a consented window into medication adherence history and relevant measurements; licensed clinician may use them in a real clinical consultation. Any medication change is an explicit **clinician-authored proposal/order** subject to identity, licensing, jurisdiction, patient acknowledgement and regulatory workflow; no backend algorithm autonomously adjusts medication. A medication schedule remains versioned, traceable, independently confirmed and governed by ADR-003.
4. Multiple professionals: patient has one account and may share different categories and scopes with different providers concurrently. Each relationship is independent, scoped, revocable and auditable.
5. Caregiver: family access and professional access are separate grant classes. Never assume a caregiver can share a dependent's information to a trainer or doctor without lawful authority.

## Fundamental data-ownership and authorization model
- Individual/patient remains the controlling data subject; professional workspace cannot claim the underlying health record or unilaterally enlist users.
- Identity account vs managed profile vs independent patient account vs professional organization/workspace are distinct objects. Track verified profession/license, organization staff roles and practice jurisdiction separately from self-declared profile labels.
- Relationship: `professional_relationship` with `subject_profile_id`, `professional_user_id`, `workspace_id?`, `purpose`, `role`, `status`, `invited_by`, `requested_at`, `accepted_at?`, `expires_at?`, `revoked_at?`, `consent_version`.
- Grants: scoped by data categories (medications, dose events, measurements, body measurements, nutrition entries, activity, medical documents), actions (read_summary, read_detail, write_professional_plan, comment, export, request_checkin), profile, effective period, history window, professional and purpose.
- Default deny for **all** clinical and medication detail access; profile management does not confer blanket professional data rights. A professional invitation is NOT a grant until patient provides deliberate informed authorization. Consent withdrawal/revocation must propagate to backend queries, exports, saved dashboards, notifications and caches.
- Service-specific enforcement at owning Medication/Records/Plan services; BFF and workspace views cannot override field-level/row-level grants. Record actor, subject, organization, purpose and export/access audit events without raw PHI in ordinary application logs.
- Personnel changes and organization exit must revoke staff-level access without dissolving user ownership. Query filters must not permit cross-patient leaks; exports are separately authorized and encrypted where applicable.
- Special cases: minors, guardianship, emergency/break-glass access, incapacity and shared phones are **explicitly deferred/open**, not implicit permissions.

## Proposed in-app communications
Future messaging between the member/patient and their coach, dietitian, physician or permitted caregiver should use one **permissioned messaging module**, not separate apps or automatically exposed SMS. See [ADR-019](../decisions/ADR-019-PERMISSIONED-MESSAGING.md). Every thread requires an active, permitted relationship; no shared patient-data permissions are implied by messages. Clinical messages, encrypted retention and exports remain gated.

## Domain separation
- `MedicationPlan`: authority under ADR-003 and draft ADR-009, distinct from provider-suggested therapy. Preserve immutable schedule versions and recorded dose events.
- `ProfessionalRecommendation`: authored suggestion with rationale/source/expiry and recipient-specific approval state; not directly executable against medication schedules.
- `ClinicianOrder` / prescription workflow: future regulated module only if justified, authorized and legally verified; clinician identity/license and prescription validity cannot be inferred from an account checkbox.
- `NutritionPlan`: date-bounded goals/meals/adherence/check-in schedule; no automatic diagnosis or clinical treatment assertions.
- `TrainingPlan`: coach sessions, exercise instructions, completion logs and progress checks; safety warning/contraindication workflow reviewed before medicalized use.
- `Measurement`: source, exact units, provenance and times; trends never misrepresent raw data as clinically validated.
- `ProfessionalMessage` and consultation notes: future regulated messaging module, retention and encryption still OPEN; do not reuse general push/email to transmit sensitive records.

## Workflow and safety state machine (candidate, not frozen)
`invited -> pending_patient_consent -> active -> suspended|expired|revoked`.
Profession-specific plan `draft -> shared_for_review -> accepted|declined -> active -> completed|cancelled`.
Medical regimen amendment is a separate clinician-authored review and patient-confirmed process; do NOT automatically treat 'accepted' as a valid prescription, do NOT rewrite prior adherence records, and do NOT autonomously prescribe. Actual clinical responsibility and prescribing legality require verified professional identity, registration and independent counsel/regulatory review.

## Architecture integration
- Patient-facing mobile app and a future professional dashboard (web or mobile) consume role-scoped BFF endpoints; professional portal release timing OPEN.
- Proposed logical bounded contexts: Professional Directory & Verification, Organizations/Workspaces, Relationships & Grants, Nutrition/Training Programs, Clinician Orders (deferred), Messaging (deferred) and Audit. **Not** a demand for one Kubernetes Deployment per context.
- Identity owns login; Profiles owns relationships or delegates to a future Authorization service after evidence; Medication and Records own health data; Notification (ADR-015) delivers generic opt-in prompts; Integration (ADR-016) records source permissions separately from professional grants.
- Service-to-service authorized decisions must carry actor and subject separately, purpose, scope, grant version, expiration and bounded tokens; don't trust user-supplied role fields.
- Treat permission revocation as a high-priority event, require online policy recheck for cloud reads and exports, prevent stale search caches, acknowledge offline already downloaded data cannot be forcibly recalled.
- Multi-tenant boundary: one coach/clinic business must never read another business's clients. Professional user may hold multiple independently constrained roles.

## Future API design questions (no route or payload frozen)
Invitation send/accept/decline; list patient-professional relationships; grant scope edit/revoke; time-limited case access; patient access ledger; professional roster cursor pagination; check-in requests; nutrition/training plan versioning; clinician review proposal. OpenAPI 3.1 per operation, stable enum/error schemes, rate limits, idempotency and ownership tests before owner API approval. Keep clinical advice distinct from general coaching outputs.

## Privacy, legality and business limits
Medical data sharing and clinical professional activity may trigger Türkiye KVKK special category obligations and Azerbaijan personal-data, healthcare professional and medical-device/prescribing rules. **All specific legal requirements and professional registration pathways TO BE VERIFIED by qualified counsel and official sources before live rollout**. Health-related coaching data can itself be sensitive. Business model (professional subscriptions, per-seat/per-patient, clinic agreements) OPEN. Emergency service, diagnosis, automatic dose change, unlicensed prescribing and clinician claims are excluded absent separate verified legal/clinical authorization.

## Research to validate
- Interview actual dietitians, coaches, physicians and users (including informed consent willingness), rather than assuming these personas want the same dashboard.
- Determine top requested metrics and smallest useful patient-authorized share for each role, history window, comparison view, reporting/export constraints and communication needs.
- Evaluate workflow fit and willingness to pay before adding medical-provider complexity.
- Prototype with entirely synthetic patient data and adversarial permissions tests (cross-organization leakage, staff departure, revoked member, exported record, multiple professionals, expired grants, unauthorized medical plan edit).
- Define professional verification, E2EE sharing/recovery (ADR-010), data residency, retention, incident response and user-access logs before real data.

## Decisions and boundaries
This is **PROPOSED / POST-MVP RESEARCH ONLY**. Accepted ADR-001 initial single-user focus and later family grants, ADR-003 authoritative medication schedules, ADR-012 microservices + Kubernetes, ADR-015 notifications and ADR-016 integrations remain unchanged. No code, clinical prescribing feature, professional verification or clinical data sharing is approved.

## Reversal cost / evidence
Low while documentation-only; high after professional grants, clinical records and billing become live. Require demonstrated professional demand, secure consent and a defensible regulatory route before accepting a release phase.
