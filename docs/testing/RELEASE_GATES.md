Status: DRAFT (gates remain OPEN/BLOCKED)
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Release gates

- GATE-00 OPEN: Scope, data classification, consent, threat model, retention, regional legal review.
- GATE-01 OPEN: Medication schedule invariants, DST/timezone tests, duplicate notifications, manual verification, no automatic dose change.
- GATE-02 OPEN: Apple HealthKit entitlements, supported data types, consent UI, device testing.
- GATE-03 OPEN: Encryption decision, conflict handling, restore, deletion, auth, audit and penetration review.
- GATE-04 BLOCKED: Private TestFlight with real health data only after 00–03 PASS and explicit MASTER GO.

A prototype with synthetic data may proceed without claiming these gates are passed.

## Mandatory proposed validation additions
- [B4 / GATE-01] Test iOS pending local notification limits (64 reported; verify current behavior and repeating notification accounting); 6 medicines x 3/day, multi-day app inactivity, kill/reboot, notification denial and no guarantee of background rescheduling. Test Android Doze, exact alarm eligibility/permissions and Play policy (to be verified).
- [B5 / GATE-00, GATE-03] Test chosen iOS file protection class, lock-state Keychain access and encrypted DB behavior during background notification actions. Do not assume `NSFileProtectionComplete` works while device locked.
- [B6 / GATE-00] Validate generic lock-screen preview default and opt-in display of medication detail, including notification privacy and accessibility.
