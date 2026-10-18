# DEPOLEDGER Documentation

> **Status:** These documents describe the proposed target architecture and requirements. They are not evidence that a capability is implemented, legally approved, or production ready. Confirm each statement against approved product, legal, security, and operational decisions before implementation.

## Documentation map

### Requirements
- [Business requirements](requirements/business-requirements.md) — goals, scope, actors, business controls, and acceptance criteria.
- [Functional requirements](requirements/functional-requirements.md) — testable platform capabilities and requirement IDs.
- [Non-functional requirements](requirements/non-functional-requirements.md) — security, resilience, privacy, audit, and operational quality attributes.
- [Legal and regulatory considerations](requirements/legal-and-regulatory-considerations.md) — review questions and approval evidence; not legal advice.

### Architecture
- [Architecture overview](architecture/overview.md) — system boundaries, layers, and technology responsibilities.
- [Domain and Daml design](architecture/domain-and-daml-design.md) — bounded contexts, contract concepts, authorization, and modeling constraints.
- [Data and financial model](architecture/data-and-financial-model.md) — authoritative sources, logical entities, money, and invariants.
- [Architecture decision records](architecture/decisions/README.md) — open decision backlog and ADR process.
- [Daml implementation plan](architecture/daml-implementation-plan.md) — dependency order, security constraints, and verification gates for contract work.

### Workflows
- [Issuance](workflows/issuance.md)
- [Transfers](workflows/transfers.md)
- [Redemption and withdrawal](workflows/redemption.md)
- [Maturity and servicing](workflows/maturity-and-servicing.md)
- [Workflow state and recovery](workflows/state-and-recovery.md)
- [Treasury and interbank funding](workflows/treasury-and-interbank.md)

### Security and operations
- [Authorization, security, and privacy](security/authorization-and-privacy.md)
- [Integrations and reconciliation](operations/integrations-and-reconciliation.md)
- [Operational controls and runbook guidance](operations/operational-controls.md)
- [Testing and release assurance](operations/testing-and-release.md)

## Source and change control

The top-level [Readme.md](../Readme.md) is the architecture concept and project overview. These documents organize proposed requirements and target-design references; they do not imply implementation or approval. When requirements change, update the relevant requirement IDs, workflows, acceptance tests, and architecture decision records together. Record unresolved choices as explicit decisions rather than treating diagrams as implementation specifications.

## Requirement status vocabulary

- **Proposed:** described as a target requirement, not yet approved.
- **Approved:** formally accepted by the accountable business/control owner.
- **Implemented:** present in code or configured service.
- **Verified:** supported by test or review evidence.
- **Production-approved:** approved through the institution's production governance.

A requirement can be implemented without being production-approved.