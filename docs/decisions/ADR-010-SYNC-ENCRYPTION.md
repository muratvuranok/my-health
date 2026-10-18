Status: DRAFT — OPEN; MASTER decision required
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-010 — Sync architecture and encryption modes

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
