Status: PROPOSED — module pricing and commercial packaging NOT approved
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

> **BACKLOG — does not shape MVP data model, endpoints, tests, deployments or release scope.** This is a post-MVP hypothesis only. Interview evidence and explicit owner GO are mandatory before promotion. Module and messaging expansion cannot silently alter accepted ADR-001 personal-first scope.

# ADR-018 — Optional modules, entitlements and monetization

## Proposal
My Health remains **one application, one account system and a shared personal-health data model** (accepted ADR-017). Users select their purpose at onboarding and may later add, remove and switch modules without creating a new identity. Optional modules could be purchased individually, bundled in subscription plans, or included in workspace/organization plans. Whether any module is paid, its price and release date remain OPEN. No automatic production billing or client-side purchase trust.

## Candidate offering — research hypothesis, not final packaging
| Module | Example capabilities | Purchaser hypothesis |
| --- | --- | --- |
| Core personal health | Medication tracking, essential reminders, personal measurements, privacy controls, export/deletion | Free/basic tier |
| Fitness | Weight/body measurements, progress dashboards, exercise logs, check-ins | Individual or coach |
| Nutrition | Weight, meal diary, diet goals and plans, selected trends | Individual or dietitian |
| Caregiver / Family | Consented family tracking and coordination | Family plan |
| Trainer workspace | Roster, 50+ member check-ins, training plans, approved measurement access | Coach / gym |
| Dietitian workspace | Consented clients, nutrition plans, selected measurements | Dietitian / practice |
| Physician workspace | Clinical follow-up, explicitly permissioned medication adherence and relevant measurements | Verified clinic/clinician; separate legal gate |
Additional module types should extend a reviewed catalog, not require new applications or arbitrary downloaded executable plugins. These are **not MVP commitments**.

## Architectural invariants — separate five different concepts
1. **Module definition:** stable `module_key`, display category, supported versions, dependencies, country/platform availability and feature status; trusted first-party features.
2. **Entitlement:** economic right to use an optional capability (free, trial, purchased, organization-issued). This controls product availability only.
3. **Purpose and workspace role:** a user can hold personal and multiple professional contexts; workspace admin is not patient-data superuser.
4. **Professional eligibility:** identity/licensing verification for regulated functions; **money cannot buy clinical permissions**.
5. **Patient consent and data authorization:** revocable data-category, action, profile, purpose, recipient and duration grants, checked at every data-owning service. Subscription alone cannot reveal health records.

## Candidate data boundaries, not frozen schemas
`module_catalog` (`module_key`, `version`, `availability`, `dependencies`).
`product_plan` (`plan_key`, `country`, `currency`, `billing_period`, `module_keys`).
`subscription` (`owner_type:account|workspace`, `owner_id`, `provider`, `status`, `period_start`, `period_end`, `grace_until?`).
`entitlement_grant` (`scope`, `module_key`, `source`, `effective_at`, `expires_at?`, `version`).
`module_preferences` (visible/installed/enabled locally or in workspace; distinct from subscription).
`billing_provider_event` (signed origin, event ID, timestamp, idempotent processing).
The word “install” means enable a trusted UI feature; no unreviewed dynamic-code loading.

## Activation and cancellation flows
- Onboarding captures **intent/purpose**, not automatically a paid purchase; list recommended modules with clear pricing and trial terms.
- A user can enable multiple modules; dependent features show missing prerequisites rather than silently requesting broad health permissions.
- Confirmed payment grants module entitlement; never trust a client `is_paid` flag or unsigned webhook. Review App Store/Google Play billing rules and regional tax obligations before implementing.
- On cancellation/expiry/refund/downgrade, premium *new* actions may become read-only by disclosed policy. **Do not erase or hold personal health data hostage.** Core reminders, privacy controls, user export/delete and safety-critical access remain available.
- Professional subscription cancellation must not silently delete client records, create continued unauthorized access, or disable the client's own account.
- Handle renewal delay, provider failure, out-of-order/duplicate notifications, restoring purchases, family sharing eligibility, account switching, multiple workspaces, offline cache expiration and grace windows.
- Payment processors receive only minimal opaque IDs, never diagnoses, medication names or measurement details.

## Pricing options to test — OPEN
A. Free core + individually purchased add-ons.
B. Free core + Personal Plus (Fitness/Nutrition bundle) + Family + Professional tiers.
C. Free personal tracking; professionals pay by practitioner seat or active consenting member.
D. A hybrid bundle + discounted add-ons.
Do not approve prices, paywall placement or regional availability from this ADR. Evaluate affordability in Türkiye/Azerbaijan and actual willingness to pay. Essential safety reminders never become an upsell.

## Architecture compatibility
One public API/gateway with purpose-specific frontend module registry; Go services can remain separately deployed under accepted ADR-012 + Kubernetes. An Entitlement/Module Catalog logical context may be added later; **not** one microservice/pod per module. Backend capabilities check commercial entitlement **and** separately verified role/profession **and** patient grant where needed. Module flags and app navigation are UX, not security.

## API contract topics before freeze
Candidate `GET /v1/modules`, `GET /v1/me/entitlements`, `GET /v1/workspaces/{workspace_id}/entitlements`, module configuration, purchase initiation and verified server-to-server billing events; none approved as final endpoints. Define entitlement version, 402/403 treatment, expiry and cache consistency, invoice identifiers, rate limits, trial/refund states, request/response and cross-platform purchase restoration. OpenAPI 3.1 with compatibility and adversarial tests required.

## Controls, owner gates and evidence
Real revenue, subscriptions, professional medical functionality and user data sharing require separate approval, regulatory assessment and provider onboarding. Research Apple/Google store billing and commerce policies using current official sources; details **TO BE VERIFIED**. Prototype using synthetic accounts and prices only. Test 50-client workspace isolation, expired membership, a paid coach without patient grant, unauthorized clinical module use, downgrade/export preservation, and duplicated payment events.

## Reversibility
Pricing packaging can change before sales; entitlements, subscriptions and purchased customer expectations become hard to migrate after launch. Keep identity, data ownership and consent architecture independent of commercial packaging.

## Decision
**PROPOSED**. This document does not approve any price, premium feature, change to initial MVP or modification of accepted ADR-001/002/003/012/017.
