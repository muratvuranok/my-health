Status: PROPOSED — nutrition module detailed scope; no implementation authorization
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

> **BACKLOG — does not shape MVP data model, endpoints, tests, deployments or release scope.** This is a post-MVP hypothesis only. Interview evidence and explicit owner GO are mandatory before promotion. Profession-specific features cannot silently alter accepted ADR-001 personal-first scope.

# FUTURE-003 — Dietitian-authored meal plans and daily nutrition tracking

## Product concept
Within the **single My Health application** (ADR-017), a dietitian can build a structured nutrition plan for a consenting client. The member receives the plan in their own My Health Nutrition module and can see **what to eat on each day, at which meal, in what portions**, optionally mark meals completed or substituted, and communicate relevant progress to the dietitian if separately permissioned. This is analogous to the clarity of a medication schedule, but **nutrition and medication are distinct domains**: meal plans cannot change prescribed medication instructions and never imply diagnosis or medical treatment.

## Primary user journey
1. A dietitian signs into an existing My Health account, selects Nutrition / Dietitian Workspace and meets separately reviewed professional eligibility requirements.
2. The client receives an invite and accepts a limited professional relationship and data-sharing scope. Dietitian cannot view medications, measurements or food records before the appropriate grant.
3. Dietitian creates a reusable template or client-specific plan: 7-day schedule, daily meals, optional snacks, meal times/time windows, portion sizes, units, preparation notes, allowed alternatives and clinically relevant cautions entered by an authorized professional.
4. Dietitian previews and **publishes version 1** to the selected consenting client. The client sees each day in chronological order and a concise daily schedule.
5. Client marks `eaten`, `partially_eaten`, `skipped` or `substituted` as explicit self-reports, with optional notes. **No event is interpreted as medically verified consumption.** If the user doesn't log a meal, display `not_logged`, not "skipped".
6. Dietitian views a consent-limited weekly adherence summary and records an updated plan for future effective dates; the original version and previous self-reports remain immutable historical records.
7. Client can revoke data sharing or leave the professional relationship; plan history access, exports, and continued use must follow explicit licensed-content, privacy and subscription policies, none of which are yet selected.

## Candidate module screens
- Dietitian workspace: clients roster, pending invitations, template library, draft meal-plan builder, weekly calendar, plan version history, selected client progress (only granted data), check-in requests and export previews.
- Client Nutrition module: Today's meals, 7-day calendar, meal details and portions, completion/substitution logging, optional shopping list derived from confirmed plan, progress timeline, reminder preferences and data-sharing controls.
- Accessibility: clear language tr/az/en, large typography, item alternatives, recognizable units and offline read of a previously fetched plan with explicit sync state.

## Proposed domain model — field names and payloads not yet frozen
- `nutrition_plan`: `plan_id`, `subject_profile_id`, `author_professional_id`, `workspace_id`, `title`, `goal_description?`, `starts_on`, `ends_on`, `plan_timezone`, `status`, `current_version`, `created_at`.
- `nutrition_plan_version`: immutable version id, `published_at`, `effective_from_local_date`, `template_id?`, `client_acknowledged_at?`, `change_reason?`.
- `meal_slot`: `day_offset` or ISO local date, `meal_type` (breakfast/lunch/dinner/snack/custom), `scheduled_local_time?`, `time_window?`, `display_order`.
- `meal_item`: `food_display_name`, `portion_quantity_decimal?`, `portion_unit` (g, ml, serving, piece etc.), `preparation_notes?`, `optional_alternatives[]`, `allergen_annotations?`. Food IDs and nutrient databases remain OPEN; do not invent verified nutrient counts.
- `meal_adherence_event`: `event_id`, `plan_version_id`, `meal_slot_id`, `actor_id`, `client_event_id`, `reported_status`, `reported_at`, `actual_consumption_at?`, `substitution_details?`, `notes?`. Do not overwrite prior entries; corrections are versioned or compensated.
- `nutrition_checkin`: weight or measurement reference if separately granted, reflection note, client-entered photos only after independent privacy/retention review (not in MVP scope).
- `nutrition_template`: reusable coach-owned draft **without client PHI**; ownership, sharing, intellectual property and revocation OPEN.

## Sample weekly meal timetable (entirely synthetic, not dietary guidance)
| Day | Meal | Example UI |
| --- | --- | --- |
| Monday | Breakfast | "Meal A" · 08:00 · plan details |
| Monday | Lunch | "Meal B" · 13:00 · plan details |
| Monday | Dinner | "Meal C" · 19:00 · plan details |
| Tuesday–Sunday | Configurable slots | Selected menu and serving descriptions |
The app must display a professional's original instructions accurately. It must not derive recommended calorie targets, prescriptions or supplements from health measurements on its own.

## Separate trust boundaries and clinical safety
- The creator's self-reported `dietitian` purpose is **not proof of license**, and role/payment entitlements do not grant access to a patient. Professional validation and country scope require later legal research.
- The individual/client authorizes relationship and **specific** data sharing. Weight, medication list, allergies, diabetes status and diagnoses are private by default and require separate category/purpose permissions. Never automatically expose medication or clinical records so a dietitian can adapt a menu.
- A medical nutrition therapy workflow, allergy management, pregnancy, pediatrics and conditions requiring clinician review may involve additional regulated constraints. These are not automatically available in the consumer Nutrition module.
- Nutrition plans must never overwrite medication schedules or be presented as medication-dosing advice. The user must be able to see which professional authored each plan and its effective date.
- Patient-facing meal reminders are separate from medication alarms; prioritize essential medication safety under ADR-015. Reminder timing is best effort and subject to OS limitations.
- Account/workspace entitlement (ADR-018 PROPOSED), professional role and client-granted data access are independent. If subscription expires, do not delete personal logs or impede export/deletion and consent withdrawal.

## Offline, edits and synchronization
- Client can view downloaded active schedule offline. Log meal actions with idempotent `client_event_id`; on reconnection reconcile against the immutable `plan_version_id` and keep conflicting plan edits visible.
- Dietitian edits create a **new future-effective version**, not retroactive replacement of past consumed meals. Same-day change requires explicit presentation to member with clear old/new comparison.
- Recurrence/time semantics: daily/weekly plans anchored to local calendar dates and IANA timezone; travel, daylight saving, daylight schedule gaps and user-selected notification timing need explicit policy.
- A missed meal entry should never auto-generate a clinical claim ("patient did not eat"). Absence of a log means only `not_logged`.
- Bulk assignment to 50+ clients is possible conceptually, but every assignment requires a valid relationship and purpose-grant and must not leak client information via templates.

## Candidate API groups — DRAFT, NOT APPROVED
- `POST/GET /v1/workspaces/{workspace_id}/nutrition/templates`
- `POST/GET /v1/workspaces/{workspace_id}/nutrition/plans` with client-ownership and consent checks.
- `POST /v1/nutrition/plans/{plan_id}/publish` (If-Match, idempotency).
- `GET /v1/profiles/{profile_id}/nutrition/plans`
- `GET /v1/profiles/{profile_id}/nutrition/today?local_date=YYYY-MM-DD`
- `POST /v1/profiles/{profile_id}/nutrition/meal-events` with client mutation ID.
- `GET /v1/workspaces/{workspace_id}/clients/{client_id}/nutrition/progress`, permissioned and bounded.
These names are placeholders. Every operation needs OpenAPI 3.1 typed request/response, error envelope, authz matrix, patient/grant scope, pagination, rate limits, idempotency, conflict and retention tests before a contract freeze.

## Release strategy, validation and alternatives
1. Synthetic clickable plan editor and seven-day client viewer.
2. Dietitian interviews on how they compose/edit weekly menus, templates, alternatives, portions and instructions.
3. Client usability: daily menu visibility, clear 'not logged' interpretation, offline availability, change notifications and plan acceptance.
4. Threat and performance tests: 50+ clients, template reuse, wrong patient plan, revoked relationship, plan edit during offline meal logs, role changes, export and account deletion.
5. Commercial tests whether Nutrition is personal add-on, dietitian-paid workspace or bundle. No prices approved.

**PROPOSED / POST-MVP until explicit owner GO.** Preserves ADR-001 initial personal scope, ADR-003 medication safety, ADR-012 Kubernetes microservices, ADR-017 one-app architecture and ADR-018 proposed modular monetization. No code, medical data, production accounts or published dietary recommendations approved.

## Reversal cost
Low while documented; high after users store diets and logs. Preserve immutable plan versions, consent granularity, record provenance and separable nutrition-vs-medication models now.
