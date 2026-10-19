Status: REJECTED AS CURRENT DEPLOYMENT ALTERNATIVE — retained for historical comparison
Date: 2026-10-18
Owner: My Health MASTER
Owner decision: 2026-10-19 — owner selected ADR-012 microservices + Kubernetes instead of ADR-013 modular-monolith-first. No reversal of ADR-012.
Supersedes: None
Superseded by: None

# ADR-013 — Product-first deployment alternative

## Proposal
Build one Go modular monolith with package boundaries identity/profile/medication/records/sync/notification, containerize it, and learn Kubernetes through local k3d deployments (K0–K3). Split services only after measured operational/scaling/team requirements.

## Alternatives and trade-offs
Accepted ADR-012 deploys a small number of independently deployable microservices with K8s. That supports direct service operations learning, but adds cross-service consistency, distributed tracing, rollout and credential complexity. This amendment reduces early overhead but changes deployment topology.

## Authority
**NOT SELECTED by owner on 2026-10-19.** ADR-012 microservices + Kubernetes remains accepted and authoritative. This is a historical alternative, not implementation authorization. A new explicit owner amendment is required to reconsider this alternative.

## Evidence to change
Interview velocity, iteration lead time, measurable independent scaling needs, operational overhead, user security and recovery tests; reversible at package boundaries but extraction is never free.
