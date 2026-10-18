# MY HEALTH — MASTER v1.2

Date: 2026-10-18. Product: My Health (working brand, trademark and store availability not yet checked). Repository: muratvuranok/my-health (private).

## Authority and boundaries
MASTER owns product strategy, scope, architecture decisions, release gates, priorities and acceptance. Lili AI orchestrates implementation through separately authorized tasks. This repository is NOT the Lili AI platform repository.

## Accepted decisions
- STR-001: Validate commercial demand in Türkiye and Azerbaijan before global expansion.
- ADR-001: Initial primary user is one individual; family authorization designed now but released later.
- ADR-002: Encrypted local storage plus controlled cloud sync.
- ADR-003: Verified medication schedule in My Health is authoritative; HealthKit imports never silently overwrite it.
- ADR-004: Private TestFlight alpha before public release.
- IOT-001: Modular device integration planned post-MVP, initially through HealthKit; custom ESP32/Arduino R&D later.

## Unresolved decisions
- Cloud data model: server-readable encryption versus end-to-end encryption. No implicit approval.
- Exact inclusion date of web dashboard.
- Country-specific legal and hosting requirements, consent and international transfers.
- Commercial pricing, segment and brand clearance: hypotheses only.

## Release gates
GATE-00 scope/privacy/security; GATE-01 medication correctness; GATE-02 Apple integration feasibility; GATE-03 sync and recovery; GATE-04 TestFlight release. All OPEN; TestFlight BLOCKED until prerequisites pass.

## Rules
No production deployment, real medical data ingestion, destructive operations, medication dose recommendations or expansion of scope without explicit MASTER GO. Every implementation task requires evidence: PR, tests, security review and handoff report. Do not treat a conversation summary as verbatim archive.
