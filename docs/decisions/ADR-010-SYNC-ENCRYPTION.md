Status: DRAFT — OPEN; MASTER decision required
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-010 — Sync architecture and encryption modes

## Owner deferral confirmation — 2026-10-19
The owner explicitly chose **"Sunucu şifreleme: ADR-010 açık kalsın"**. Neither E2EE nor server-readable/KMS has been selected. ADR-014 local-only alpha sequencing is accepted and therefore avoids premature server encryption choices; **it is not a final encryption decision**. ADR-002 encrypted local storage + controlled future cloud sync continues to apply. Server schema, hosting and full sync API contract remain NOT FROZEN.

## Decision OPEN (B2, B7, B8)
**Option A: server-readable application data + encryption at rest/envelope KMS.** Enables server-side validation, conflicts, search, exports and family sharing; imposes breach/insider and jurisdictional risks.

**Option B: end-to-end encryption.** Stronger protection from cloud compromise, but device loss/recovery, multi-party grants, web, server validation, offline conflict processing and key rotation become hard; require threat model and prototype.

**Option C: local-only alpha while research continues.** Minimizes immediate cloud exposure but postpones cross-device value and requires safe eventual migration; see PROPOSED ADR-014 and accepted ADR-002.

## Unanswered sync contract
Choose data owner for mutation processing, per-resource typed oneOf payloads, partial batch semantics, idempotency TTL, 409 conflict mapping, monotonic cursor, tombstones, permission revocation/reset, delete propagation, multi-device replay, atomicity per service, retriable timeout outcomes and service credentials.

## Hosting/legal
Türkiye KVKK, Azerbaijan law, residency/transfer and recovery policy require jurisdiction-specific qualified verification. No region, KMS provider or production cloud selection approved.

## Authority
ADR-002 remains ACCEPTED at principle level. Encryption mode, hosting and any postponement are OPEN. Only explicit owner decision can close them.
