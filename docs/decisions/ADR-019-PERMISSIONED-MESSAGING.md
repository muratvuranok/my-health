Status: PROPOSED — secure in-app communications module; no live messaging or clinical approval
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

> **BACKLOG — does not shape MVP data model, endpoints, tests, deployments or release scope.** This is a post-MVP hypothesis only. Interview evidence and explicit owner GO are mandatory before promotion. Module and messaging expansion cannot silently alter accepted ADR-001 personal-first scope.

# ADR-019 — Permissioned in-app messaging across My Health modules

## Product intent
One My Health account (accepted structural ADR-017) can participate in carefully authorized conversations with a dietitian, trainer, physician, patient/client or permitted caregiver. The user can message the relevant professional from within the same app, and professionals can reply from their workspace. This is a **shared platform capability**, not a separate messaging app per persona. Messaging is PROPOSED for a future release and **must not** be treated as existing clinical advice, secure clinician service, emergency triage or production-ready encrypted chat.

## Examples
- Dietitian and consenting client discuss a specific week's meal plan, clarifications and check-in; messages may reference `nutrition_plan_version_id` without automatically attaching health data.
- Coach and member discuss training schedule or measurement-entry request; a coach has no access to medication information by virtue of conversation membership.
- Physician and patient may communicate about observed measurements and adherence **only within a separately verified clinical workflow and lawful eligibility**; medication prescription/change must use explicit regulated clinical action, not free-text message interpreted as automatic order.
- Caregiver messaging requires a legal relationship and explicit permission, independent from professional or workspace membership.
- Member may have separate conversations with multiple professionals; professionals cannot see each other's conversation or discover other relationships without authorization.

## Identity, permission and conversation lifecycle
- A `conversation` has a subject profile reference, purpose/domain, initiating relationship/grant ID, permitted participants, status, created/closed timestamps and optional plan reference. Use conversation-specific authorization: workspace admin role or paid module entitlement **alone never permits reading messages**.
- Participants must be existing authenticated identities. First contact requires an accepted care/workspace relationship and messaging scope; no cold direct messages to arbitrary accounts. Invitation acceptance and recipient blocking/reporting workflow to be defined.
- Define participant-specific permissions: `send`, `read`, `reply`, `attach`, `invite` (default false), `export` (default restricted), `moderate` (limited and audited).
- States candidate `pending_consent -> active -> paused -> closed|revoked`; expiry and relationship revocation stop new deliveries and future retrieval, with explicit retention/history policy and lawful exception review.
- Patient maintains control of their data; **message records are jointly authored communications**, so retention, subject access and deletion may involve multiple lawful interests. No blanket promise that one user's delete erases the other party's legally required record. Decide before live use.
- Professional role verification and country/practice authority are separate from user self-declaration, payments and UI selection. Never assume patient consent to a professional relationship implies medication, document or full history access.

## Messaging domain and delivery architecture
- **Messaging service** owns conversation membership, messages, ordering and moderation/audit metadata. It does not own medications, nutrition plans, measurements, contact information or clinical orders.
- **Notification service** (proposed ADR-015) consumes `message.created.v1` delivery intents and sends generic push/email alerts (e.g. "You have a new message"), without message text, health data or names by default. Notification attempts and delivery status are not authoritative message persistence.
- Use server-assigned per-conversation monotonically increasing `sequence_number`; `client_message_id` for idempotency/retry; persistent `message_id` and `created_at`. Device clocks never determine canonical order.
- Real-time path: authenticated WebSocket **or** event stream, plus HTTP APIs for history/send/read; choice OPEN until OS/mobile/K8s load and reconnection tests. Offline delivery uses pagination/cursors and retry; presence/typing indicators deferred.
- Data must be encrypted in transit and under the selected approved rest/E2EE design. **ADR-010 E2EE/server-readable remains OPEN**: full-text search, attachments, multi-device key recovery, abuse reporting, moderation, exports and legal record retention depend on that decision.
- No PHI in queue topic names, tracing spans, metrics labels, analytics, push payloads, logs or raw exception text. Use opaque message/conversation references and minimum metadata.
- Attachments (photos, PDFs, clinical records) are **DEFERRED** pending file security, signed URL/TTL, antivirus, MIME verification, quotas, storage residency, encryption, permission and data-retention decisions. No arbitrary third-party URLs or access-token sharing.
- Message edit/delete semantics must preserve audit/history appropriate to context; a user cannot secretly mutate a clinician's record or medication event.

## Contextual UI and scope
- A shared **Messages** entry in My Health; context selector filters Personal, Coach, Dietitian, Clinic workspaces without switching identity.
- One-to-one initially recommended; team inbox, multi-party conversations, broadcast, bots, group chat and patient community channels are DEFERRED until distinct privacy/governance review.
- Thread may have `context_type` (`nutrition_plan`, `training_plan`, `care_followup`, `general`) and `context_ref`, but reference visibility is checked independently. Sharing a plan link or text is not permission to read the underlying record.
- Unread counters and notification preferences per context; quiet hours and communication availability. Explicitly distinguish `sent_to_server`, `delivered_to_device` (if verifiable) and `read` states. Do not promise guaranteed delivery/read or an emergency SLA.
- Block/mute/report and professional out-of-office boundaries; spam/harassment protections and a clinical escalation disclaimer must be considered. **No emergency service:** direct emergency symptoms to appropriate urgent care with non-diagnostic UX.

## Candidate resource/contract outline (NOT FROZEN)
- `conversation`: `id`, `context_type`, `context_ref?`, `subject_profile_id?`, `relationship_id`, `workspace_id?`, `status`, `last_sequence`, `created_at`.
- `conversation_participant`: `user_id`, `role`, `joined_at`, `revoked_at?`, `last_read_sequence`.
- `message`: `id`, `conversation_id`, `client_message_id`, `sequence`, `sender_id`, `content_type`, `body` (mode-dependent), `created_at`, `edited_at?`, `redacted_at?`, `retention_class`.
- API candidates `GET/POST /v1/conversations`, `GET/POST /v1/conversations/{id}/messages`, `POST /v1/conversations/{id}/read`, `POST /v1/conversations/{id}/close`; operation-level request/response, errors, cursor semantics, optimistic concurrency and roles require OpenAPI 3.1 before GO.
- Errors include stable machine codes for `relationship_not_active`, `messaging_permission_required`, `conversation_closed`, `recipient_unavailable`, `message_too_large`, `idempotency_conflict`, `rate_limited`; externally mask cross-account existence consistently.

## Integration with modular monetization
ADR-018 modular entitlements remain PROPOSED. Basic communication access vs premium coaching/professional inboxes, quotas and storage are OPEN; cancellation cannot strand sensitive personal records, block export/deletion, or create unauthorized persistent access. **Payment does not authorize a conversation or disclose protected data.** Safety-critical clinical follow-up accessibility and any obligations require independent review.

## Abuse, privacy and operational test gates
- Strong sender/recipient authorization for every send/read; unrelated workspace admin or staff cannot browse conversations; test 50-client isolation.
- Revocation in the middle of a WebSocket session or history request; token refresh, lost phone, account unlink, staff termination and account deletion.
- Message ordering under concurrency, idempotent retry after timeout, duplicate push, event out-of-order, stale read receipt, reconnect with gaps, pagination tombstones, receiver blocked, notification privacy on locked screens.
- Rate limits, abuse/spam/reporting without dumping content into logs; redaction and data export; retention/backup restore; interoperability with new app versions; encrypted search and multi-device recovery only if ADR-010 supports them.
- Clinical and country-specific communication record obligations, consent, response-time representations, reporting needs, identity verification and professional liability **TO BE VERIFIED** with qualified local experts. No clinical messages or real user data in synthetic prototype.

## Decision, alternatives and reversal
**PROPOSED**. Alternative: external SMS/WhatsApp links (fast but weak integration/control and third-party health-data sharing risks); third-party chat SaaS (quicker delivery but data-processing, encryption, retention, vendor lock-in); native first-party messaging (control but security/operations burden). No provider or in-house implementation choice is approved. Reversal becomes expensive once clinical message histories, compliance obligations and client expectations exist. No new release GO and no changes to existing accepted ADRs.
