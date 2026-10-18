Status: ACCEPTED — architectural principles; endpoint payloads DRAFT
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-015 — Notification and messaging architecture

## Decision and scope
My Health includes **three different delivery categories**: (1) device-local medication reminders with offline scheduling and user interaction, (2) mobile remote push for non-guaranteed informational and account/caregiver notifications, and (3) transactional email for verification, sign-in/security, account actions and user-requested reports. The Notification service is an asynchronous delivery subsystem, not the source of medication schedules or the canonical record of doses. Accepted ADR-012 microservices/Kubernetes continues to govern deployment topology. No real recipient/provider use or production infrastructure authorized.

## Core design invariants
- Medication service owns schedule version and occurrence identity; client owns local reminder scheduling and reconciliation of its delivery plan. A delivered notification **is not evidence a medication was taken**. Push/email delivery is **best effort** and cannot guarantee on-time medication reminders.
- Device-local reminders must work without network; mobile OS limitations (iOS pending limits and background scheduling, Android Doze/exact alarms) must be validated on real devices, including after multiple days of app inactivity. Never assume iOS BGAppRefreshTask will run at a chosen time.
- Cross-device caregiver messages require explicit permission grants, consent and validated actor/subject separation. Future feature, not silently enabled by an owned/managed profile.
- The default lock-screen notification has **no drug name or health detail**. Sensitive preview requires explicit opt-in and platform-specific validation. Email subject/body and push payloads should contain no sensitive health information by default; deep links must reauthorize before displaying content.
- Security-critical email (verification, account recovery, suspicious login, account deletion) is classified separately from optional reminders/marketing; marketing requires distinct opt-in. User opt-outs must not disable mandatory security notices, subject to jurisdictional review.
- Never embed health records or credentials in push payloads, URL query strings, provider logs or unprotected analytics.

## Service ownership and flow
1. Domain service writes its business change and versioned outbox event **in the same local database transaction**.
2. Event publisher transfers to queue/notification consumer with at-least-once delivery; no distributed transaction assumed. Deduplicate by (event_id, channel, recipient, template_version, notification_kind).
3. Notification service loads authorized recipient routing and effective preferences, creates immutable delivery attempt state, enqueues retries with bounded exponential backoff/jitter, handles permanent failures and provider callbacks. Idempotency is not exactly-once delivery: clients may still receive duplicates.
4. Provider adapters: APNs (iOS push), FCM (Android push), SMTP/email provider (Mailpit local only). Production providers, billing, sender domains and credentials remain OPEN.
5. Device registration carries platform, token binding, owner/session, install identity, timestamps, revoked_at and minimal metadata. On logout, delete-account, token change or consent revocation, revoke routing promptly.
6. Keep bounded retention for queue payloads and delivery records; define privacy classification, erasure policy and per-country legal basis before live use.

## Message taxonomy and priority
- `security.*`: verification, login/identity change, password reset, account deletion; transactional email primarily. Separate abuse prevention and rate limits.
- `medication.reminder_local`: local-only canonical reminder; no server delivery guarantee.
- `medication.followup_optional`: push/email follow-up only after explicit enablement and documented timing/opt-in; avoid clinical urgency claims.
- `caregiver.*`: future permissioned share/invite/status events, not MVP cross-account access by default.
- `product.*`: optional service announcements; marketing separate from service communication.

## Notification policy
Store channels, category preferences, per-profile privacy flags, quiet hours/timezone and verified recipient addresses. Time-sensitive security mail may bypass quiet hours; configurable patient-safety reminders should not be suppressed without visible user confirmation. Dedup, TTL, cancellation upon rescheduling, eventual consistency on preference change and expiry before dispatch must be explicit. Never send outdated schedule reminders after archival; local pending notifications are reconciled when possible. User can view in-app notification center; read/unread state is not provider delivery state.

## Failure and protection model
Retries for transient 429/5xx/network failures; no retries on permanent bounce/invalid token; dead-letter or quarantined failures require safe observability, no PHI. Shared rate limiting across replicas and provider-specific quota/admission limits. Outbox backlog alerts, delivery latency/error metrics, notification cancel/revoke propagation, worker failover and regional outage tests. Avoid public exposure of provider token identities. Email deliverability (SPF/DKIM/DMARC) and Apple/Google provider policies to be verified for production.

## API compatibility and data minimization
External design conventions in ADR-007; full operation-level OpenAPI and examples remain DRAFT in docs/backend/NOTIFICATION_API_DRAFT.md. Never treat proposed payload fields as frozen without contract review.

## Tests and authorization
Local synthetic end-to-end from domain outbox to Mailpit/mock APNs/FCM; deterministic preference and authorization tests; duplicate event, token rotation, logout/revocation, account deletion, consent withdrawal, provider throttling, outage recovery, timezone/quiet hours, sensitive preview and real-device reminders. Real provider E2E and live health-data beta remain blocked by MASTER gates.

## Alternatives and reversal
Direct in-request provider calls rejected due to latency and retry/data-consistency risk. Push-only medication reminders rejected for unreliable connectivity/timing. Managed broker vs PostgreSQL outbox and broker technology remain OPEN. Reversal becomes costly once device registrations, delivery logs and templates are deployed; revisit with actual operations evidence.
