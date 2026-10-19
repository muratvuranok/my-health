Status: DRAFT
Date: 2026-10-18
Owner: My Health MASTER
Supersedes: None
Superseded by: None

# Lili AI → My Health implementation contract

## Role
Lili AI is the coding/test/review orchestrator for the independent My Health product. It is not the health-data owner, medical authority or autonomous release manager.

## Historical first assignment: MH-001 + MH-002 (planning only; execution requires a current task in TASKS.md)
Read MASTER.md v1.3, docs/decisions/ADR-012-MICROSERVICES-KUBERNETES.md, docs/architecture/ARCHITECTURE_AUDIT_V1.md, docs/decisions/ADR-005-MOBILE-PLATFORM.md, ROADMAP.md, docs/product/MVP_SCOPE.md, SECURITY.md, architecture and release gates. Prepare a mobile shell (platform per ADR-005) and synthetic-data interactive prototype plan. ADR-005 is ACCEPTED on 2026-10-19; the **new synthetic-only prototype planning handoff** is [MH-PROT-001](MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md). This historical assignment still is not a blanket implementation GO. Do not implement beyond the explicitly approved task scope.

## Current task authority
Read [MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md](MH-PROT-001-SYNTHETIC-MOBILE-PROTOTYPE.md) and TASKS.md. Prepare the scoped prototype implementation plan and artifacts only as specified there. Do not reactivate suspended backend handoff or bypass contract/security gates.

## Execution boundaries
- One feature branch/PR per task; never push directly to main without authorized workflow.
- No real medical data, secrets, chat transcripts, production infrastructure, deployment, store release or live health integrations without explicit GO.
- Use synthetic test fixtures. Preserve repo separation from Lili AI platform.
- Record decisions, risk, code changes, test evidence, screenshots when appropriate, commit/PR identifiers and blocked dependencies.
- Do not mark tasks ACCEPTED or CLOSED without reviewer and MASTER acceptance.

## Delivery template
Task ID; scope; files changed; tests and results; security/privacy implications; known gaps; PR/commit evidence; gate status; decision requested.
