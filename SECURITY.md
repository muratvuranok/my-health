Status: DRAFT
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Security and sensitive data handling

My Health processes potentially sensitive personal health data. Repository is temporarily **PUBLIC** for external tool review, and is **not** an approved health-data vault.

- Never commit actual medical records, names tied to diagnoses, prescriptions, health exports, tokens, private keys or chat transcripts containing personal health information.
- Use synthetic fixtures and redacted examples only.
- Threat-model account takeover, cross-user access, notifications on lock screens, lost devices, sync conflicts, recovery, backups, logs and analytics.
- Encrypt local sensitive records; minimize server data; enforce least privilege, audit access, secure deletion and explicit consent.
- Evaluate Türkiye KVKK, Azerbaijan privacy law and any applicable medical-device regulation with qualified counsel before live rollout.
- HealthKit permissions are per data type; data imported from HealthKit must not be used for advertising or sold to brokers.
- No autonomous dosage change, diagnosis or treatment recommendations.
- Independent security review and verified restore/recovery before live-data TestFlight GO.

Report vulnerabilities privately to repository maintainers; do not open public issues containing sensitive details.

## Vulnerability reporting (A8)
Use GitHub **Private vulnerability reporting** if enabled in repository Settings > Security > Advisories. **Enablement has not been verified**; until a private channel is configured, do not post vulnerabilities, tokens or health data in public issues. Maintainer must configure and verify a private reporting route before accepting external vulnerability reports.

## Device privacy defaults (B5–B6; proposed pending approval)
- Generic lock-screen notification content by default; showing medication names is an explicit per-user choice.
- Review iOS file protection and Keychain unlock states against background actions before choosing SQLCipher/key storage. `NSFileProtectionComplete` can block data access when locked; do not pick protection classes solely for convenience.
