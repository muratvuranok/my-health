Status: ACCEPTED — alpha sequencing only; real-data and platform gates OPEN
Date: 2026-10-18
Owner: My Health MASTER
Owner acceptance: 2026-10-19 — explicit owner choice "İlk alfa: Önce local-only".
Supersedes: None
Superseded by: None

# ADR-014 — Local-only mobile alpha alternative

## Accepted alpha scope and separation
An early alpha with no cloud account/sync: encrypted device-local medication plans, reminders, dose history, selected HealthKit/Health Connect ingestion where permitted, and clinician-discussion export. Use synthetic data for early prototype; real data remains gated by MASTER and release policy.

## Alternatives
Accepted ADR-002 targets encrypted local storage **plus controlled cloud sync**; this proposal changes alpha sequencing, not accepted long-term intent. Backend-first alpha and hybrid alpha remain alternatives.

## Critical privacy questions
Apple App Store Guideline 5.1.3 restrictions on personal health information and iCloud storage are **to be verified against the current official text**. Document backup behavior, device loss, export, local delete, permission withdrawal, and healthcare claims before any real-data tests.

## Authority and reversibility
**ACCEPTED for initial alpha sequencing only.** First alpha runs locally without backend dependence, auth, cloud sync, professional grants or production provider calls. This **does not supersede ADR-002**: controlled cloud sync is still the longer-term intent and ADR-010 encryption mode remains OPEN. Start with synthetic data for initial prototype; real user/health-data alpha is **BLOCKED** until GATE-00/01/02 and applicable local storage, consent, backup, export/deletion and real-device reviews, and explicit separate release GO. No blanket iCloud backup assumption. Migration from local-only profiles to future authenticated accounts must be designed/tested before enabling cloud sync.
