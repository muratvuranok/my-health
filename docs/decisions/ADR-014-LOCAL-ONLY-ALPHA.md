Status: PROPOSED — owner approval required
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# ADR-014 — Local-only mobile alpha alternative

## Proposal
An early alpha with no cloud account/sync: encrypted device-local medication plans, reminders, dose history, selected HealthKit/Health Connect ingestion where permitted, and clinician-discussion export. Use synthetic data for early prototype; real data remains gated by MASTER and release policy.

## Alternatives
Accepted ADR-002 targets encrypted local storage **plus controlled cloud sync**; this proposal changes alpha sequencing, not accepted long-term intent. Backend-first alpha and hybrid alpha remain alternatives.

## Critical privacy questions
Apple App Store Guideline 5.1.3 restrictions on personal health information and iCloud storage are **to be verified against the current official text**. Document backup behavior, device loss, export, local delete, permission withdrawal, and healthcare claims before any real-data tests.

## Authority and reversibility
**PROPOSED; no approval for scope reduction or live testing.** Evaluate whether interviews show a real cross-device need and whether safe migration from local data to synced accounts can be guaranteed.
