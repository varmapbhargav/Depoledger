# Architecture Decision Records

Use an ADR for decisions that materially affect system boundaries, financial correctness, privacy, security, or operations. ADRs record the decision and rationale; they do not replace business, legal, regulatory, or security approval.

## Status

No architecture decisions are approved by this documentation set. The topics below are open decisions to assign to accountable owners; do not treat listed options or workflow diagrams as selected designs.

## Initial decision backlog

1. Authoritative system per financial field and event.
2. Issuance and redemption ordering, reservation, and recovery model.
3. Mapping between enterprise identities, customer mandates, services, Canton parties, and Daml controllers.
4. Contract stakeholders, transaction visibility, and audit access.
5. Product/version governance and compatibility strategy.
6. Integration delivery, idempotency, and status/finality semantics.
7. Workflow orchestration technology and operational ownership.
8. Reconciliation keys, tolerances, timing, and exception ownership.
9. Key custody, signing policy, participant topology, and disaster recovery.
10. Production service objectives, retention, and evidence storage.

Create a separate numbered ADR for each decision, using [the template](template.md). Record superseded decisions rather than deleting their history.