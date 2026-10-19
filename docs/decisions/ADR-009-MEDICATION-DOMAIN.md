Status: DRAFT — OPEN; no medication semantics approved
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-009 — Medication domain and temporal semantics

## Open questions (D1–D3)
- Recurrence: fixed times, intervals, weekdays, X days on/Y days off, PRN, variable/tapered dosage, start/end, meal-related annotations, paused/resumed, travel.
- Time policy **per plan**: floating local wall clock vs anchored timezone/instant; what happens on DST gaps and repeated clock hours?
- Occurrence identity candidate: plan_id + immutable rule_version + scheduled local date/time + timezone + disambiguation fold; evaluate collisions and migration impact.
- Immutable historical plan/rule versions, future-only edits by default; audit corrections rather than overwrite dose history.
- Recorded events vs derived statuses: taken/skipped/snoozed are actions; missed/late are interpreted from schedule windows. Define taken_at, scheduled_for, user recorded_at, actor, device and idempotency.
- Plan archiving vs hard delete; retention, legal erasure, attribution and sync tombstones.
- Dose amounts as optional decimal strings, coded units, dosage form, display text and provenance; no automatic conversion or dose recommendations.
- Offline duplicates, conflicting plan edits, multi-device reminders, consent and managed profiles.

## Safety invariants
Never silently change the authoritative plan from imported device data. No medical diagnosis or suggested dose. Treat imported sample provenance separately from medication authority. Test property-based DST/travel changes and event replay.

## Decision and evidence
All enumerated models and proposed rules are **OPEN** pending domain review and owner sign-off. No API contract freeze or implementation assumption. Reversal after data migration is high-cost.
