Status: PROPOSED — product idea only; no implementation or release authorization
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

> **BACKLOG — does not shape MVP data model, endpoints, tests, deployments or release scope.** This is a post-MVP hypothesis only. Interview evidence and explicit owner GO are mandatory before promotion. Profession-specific features cannot silently alter accepted ADR-001 personal-first scope.

# FUTURE-001 — Trainer / coach workspace concept

## Single-product integration
Per [ADR-017](../decisions/ADR-017-UNIFIED-PRODUCT-MODULAR-WORKSPACES.md), this is a **module/workspace context within the one My Health product and account system**, not a separate trainer app. Coach onboarding selection is not proof of professional eligibility or permission to access client health records. This module remains PROPOSED.

## Motivation
Some individuals already report body weight, circumferences, body composition and weekly training updates to a trainer. My Health could let individuals selectively share relevant progress with a coach in one place. This is a future opportunity hypothesis, not approved MVP scope, a medical service, or permission to expose sensitive health information.

## Example workflow
- A personal trainer maintains a workspace with ~50 invited members, each with an independent My Health account.
- A member receives an invitation and can accept, decline, narrow categories, set an expiry date and revoke access.
- Member chooses specific data such as weight, waist/hip circumference, selected training check-ins and optionally particular measurements; defaults deny all categories.
- The trainer can view only consented permitted metrics and history for each individual in their roster. No global 'master' account with unrestricted access to all health records.
- Member may optionally accept an exercise plan or weekly check-in request created by the trainer; plan author/source is recorded. Coaching plans do not edit medication schedules or issue clinical dosing instructions.
- On cancellation/revocation, future reads, pending exports and notification distribution must respect the new grant state. Previously downloaded copies cannot be technically recalled; inform users and contractually constrain recipients.

## Architectural boundaries (future-compatible, not final API)
- Identity authenticates the individual professional and member; professional certification is a separate verification claim, never inferred from self-declared title.
- Organization/workspace belongs to a coach or coaching business. Multiple coaches, staff roles and 50+ memberships must be isolated by workspace and per-member grant.
- Client health data is owned by the individual member, not the coach or gym. A one-device caregiver-managed profile is different from an independently consenting member account; do not conflate these permissions.
- Permission model candidate: `subject_profile_id`, `grantee_user_id` or `workspace_role_id`, `data_categories[]`, `allowed_actions[]`, `purpose`, `grant_version`, `granted_at`, `expires_at?`, `revoked_at?`, `granted_by`, audit actor and access log.
- Distinguish view, aggregate/trend, comment, plan-write, export and notification permissions; aggregate dashboards must not silently bypass row-level or field-level grants.
- Coach-created workout plans/check-ins are a different domain aggregate from medical medication plans. A dedicated coaching module or later independently deployed service is an implementation option, **not** a change to accepted ADR-012 boundaries.
- Backend data-owning services enforce authorization on every read/write, not only gateway/UI. Tests cover enumeration/IDOR, exports, revoked membership, staff role changes, expired grants, cross-organization access, shared devices and cached/offline access.
- Record source and provenance of manually entered vs wearable measurements; do not imply clinical validity or prescribe treatment.
- Coach bulk summary should be minimal, e.g., percent of members who completed weekly check-in; explicit permission for member-specific health detail.
- Billing, package subscriptions, trainer-seat pricing and business ownership are OPEN; never create payment or medical-professional registration obligations without review.

## Candidate member-entered measurements
Body weight (decimal + unit), waist/hip/chest/arm circumference (decimal + cm/in unit), optional body-fat percentage with method/device provenance and uncertainty, training attendance/check-in, activity notes, user goals. Avoid collecting diagnoses, medications, menstrual/fertility, precise location or health documents by default. Define retention periods and allowed metrics per jurisdiction and intended purpose.

## Consent and safety prerequisites
Sensitive wellness/body measurements may be health-related personal data; Türkiye/Azerbaijan privacy and cross-border sharing implications require qualified legal review before live launch. A checkbox or workspace invite does not authorize a blanket disclosure. Explicit and revocable consent/grants, least privilege, downloadable access log and user export/delete flows needed. Staff/team visibility and minors require separate restrictions. Trainer advice must not be represented as clinical care or medical-device functionality.

## Open questions and evidence
- Is the real paying customer coach, gym, member or caregiver? Conduct interviews.
- Does the user want weekly manually entered data, HealthKit/Health Connect-imported data or both? Are coaches allowed raw samples or only weekly aggregates?
- Permission expiration, default share categories, historical data visibility, screenshot/export limits and derived trend sharing?
- Coach-to-member communication, reminders and workout plans needed for real value or should this remain tracking-only?
- Professional credential verification, safeguarding minors, legal basis for disclosure, organization administration and audit retention?
- Can we demonstrate demand with a synthetic prototype and 5–10 trainer interviews before adding backend grants?

## Decision status
**PROPOSED / BACKLOG ONLY.** Do not amend accepted ADR-001's delayed family/cross-account authorization policy; future professional sharing needs a separate explicit owner GO and permission ADR. No executable endpoints, release promise or priority change authorized.

## Reversal cost
Low during planning; high after cross-account grants and coach billing ship. Decide permissions, tenancy, export and ownership policies before implementation.
