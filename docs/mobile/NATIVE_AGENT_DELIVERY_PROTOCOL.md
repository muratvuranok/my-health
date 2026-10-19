Status: ACCEPTED — agent delivery workflow only; implementation requires separate task GO
Date: 2026-10-19
Owner: My Health MASTER
Owner acceptance: 2026-10-19 — owner chat explicitly selected one Mobile Lead agent coordinating independent iOS and Android agents working from the same specification, finishing and testing both platforms.
Supersedes: None
Superseded by: None

# Native Mobile Agent Delivery Protocol

## Operating model
One **Mobile Lead Agent** owns planning, scope and cross-platform acceptance. Two specialist implementation agents work **in parallel**:
- **iOS Agent:** Swift, SwiftUI, HealthKit, UserNotifications, Keychain, platform build/unit/UI/device tests.
- **Android Agent:** Kotlin, Jetpack Compose, Health Connect, alarms/notifications, Keystore, platform build/unit/UI/device tests.

The Mobile Lead issues **one canonical feature brief and acceptance matrix** to both agents simultaneously. Platform differences (permission flows, OS-specific APIs, lifecycle constraints and idiomatic UX) are explicitly documented as deltas, not silently forced into identical code. Each agent works in an isolated branch/worktree and owns only its platform files; the lead manages integration and shared specification changes.

## Shared feature brief — required before dispatch
Feature/task ID and owner GO; product intent and in/out of scope; intended profile and permissions; user flows, states and screen definitions; field-level API/OpenAPI version when backend endpoints are needed; fixture IDs; offline behavior; accessibility and tr/az/en labels; error-state and privacy expectations; security invariants; event/timezone rules; negative tests; acceptance evidence; dependencies and stop conditions. **If backend schemas are unfrozen, only synthetic/local UI prototype scope expressly authorized by the owner may be dispatched.**

## Parallel handoff and feedback
1. Lead reviews MASTER, ADR-005, API/gate statuses and current task permissions; refuses superseded/SUSPENDED handoffs.
2. Lead freezes a *task-level* specification version/hash for the feature (this does not imply whole API contract freeze) and assigns both agents identical behavior/acceptance cases.
3. iOS and Android agents each prepare a brief implementation plan and raise unresolved platform-specific constraints early. Lead decides UX-level deltas only within approved scope; owner resolves architectural/clinical/security changes.
4. Both agents implement, run platform-local builds and automated tests, and provide reproducible evidence with exact git SHA, device/OS or simulator/emulator version, commands and outcomes. No invented PASS.
5. Lead compares behavior against one feature parity checklist and tests/validates both outputs independently. For tests the lead cannot physically execute (device permissions/OS background), report **NOT VERIFIED** and hold the relevant gate; agent attestations alone are not acceptance.
6. CI checks + code review for each platform, security review, OpenAPI compatibility when relevant, deterministic synthetic E2E flows and no regression; only then feature state becomes review-ready. OWNER/Master gates still control merges/releases as applicable.
7. Lead publishes one consolidated report: iOS PASS/FAIL/NOT VERIFIED, Android PASS/FAIL/NOT VERIFIED, shared acceptance matrix, deviations, outstanding risks, references, next action. **Both platforms must reach required acceptance for a feature to be called cross-platform DONE.** Platform-specific staging may proceed only with separate owner scope.

## Suggested parity matrix per feature
| Test dimension | iOS Agent | Android Agent | Lead acceptance |
| --- | --- | --- | --- |
| UX flow and validation | SwiftUI tests | Compose/UI tests | Same intent and errors; native affordances permitted |
| Domain behavior | Deterministic fixtures | Identical fixtures | Event/time/unit semantics match |
| Authentication and access | Platform login handling | Platform login handling | No unauthorized profile access |
| Offline and resilience | App restart/offline | App restart/offline | No silent state loss |
| Scheduling and notifications | OS-specific real device tests | OS-specific real device tests | No false reliability guarantees |
| Health permissions/integrations | HealthKit tests | Health Connect tests | Least privilege and revocation |
| Privacy and secure storage | Keychain/locked device | Keystore/locked device | Same security invariants |
| API compatibility | Swift schema/client | Kotlin schema/client | Both agree with the same OpenAPI |
| Localization/accessibility | tr/az/en, dynamic type | tr/az/en, font scaling | Functionally accessible in both |

## Quality controls
- Do **not** assume that generating two implementations guarantees behavior parity; use shared synthetic fixtures and explicit negative acceptance tests.
- Do **not** silently alter a shared domain/API contract in one platform; lead opens a contract decision and synchronizes both branches before resuming.
- Do **not** allow iOS testing to stand in for Android testing, or vice versa. Simulator/emulator validation does not replace required real-device medication/HealthKit/Health Connect verification.
- Never send actual patient data, production credentials or signing keys to agents. Each agent uses fake data and sandbox services.
- Agent concurrency reduces duplicated execution time but **does not eliminate** second-platform build, test, device and maintenance costs; budget lead review and CI capacity.
- Health-related safety, permissions and medical-domain correctness gates cannot be waived by the lead. Lead coordinates; owner retains decisions and release authorization.

## Relationship to ADRs
Implements accepted native mobile direction ADR-005. Keeps ADR-012 Go microservices/Kubernetes and ADR-017 unified product/account boundaries unchanged. ADR-015, ADR-016 and individual API schemas stay in their documented proposal/open statuses. No coding authorized by this process document alone.
