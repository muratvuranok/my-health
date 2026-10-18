# Lili AI → My Health implementation contract

## Role
Lili AI is the coding/test/review orchestrator for the independent My Health product. It is not the health-data owner, medical authority or autonomous release manager.

## First assignment: MH-001 + MH-002
Read MASTER.md, ROADMAP.md, MVP_SCOPE.md, SECURITY.md, architecture and release gates. Prepare implementation plan for SwiftUI tr/az/en shell and synthetic-data interactive prototype. Do not implement beyond the explicitly approved task scope.

## Execution boundaries
- One feature branch/PR per task; never push directly to main without authorized workflow.
- No real medical data, secrets, chat transcripts, production infrastructure, deployment, store release or live health integrations without explicit GO.
- Use synthetic test fixtures. Preserve repo separation from Lili AI platform.
- Record decisions, risk, code changes, test evidence, screenshots when appropriate, commit/PR identifiers and blocked dependencies.
- Do not mark tasks ACCEPTED or CLOSED without reviewer and MASTER acceptance.

## Delivery template
Task ID; scope; files changed; tests and results; security/privacy implications; known gaps; PR/commit evidence; gate status; decision requested.
