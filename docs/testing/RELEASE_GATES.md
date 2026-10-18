# Release gates

- GATE-00 OPEN: Scope, data classification, consent, threat model, retention, regional legal review.
- GATE-01 OPEN: Medication schedule invariants, DST/timezone tests, duplicate notifications, manual verification, no automatic dose change.
- GATE-02 OPEN: Apple HealthKit entitlements, supported data types, consent UI, device testing.
- GATE-03 OPEN: Encryption decision, conflict handling, restore, deletion, auth, audit and penetration review.
- GATE-04 BLOCKED: Private TestFlight with real health data only after 00–03 PASS and explicit MASTER GO.

A prototype with synthetic data may proceed without claiming these gates are passed.
