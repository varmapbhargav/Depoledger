# Architecture Overview

## 1. Status and intent

This is a logical target architecture for DEPOLEDGER. It is not a deployment topology, proof of atomicity, or claim that any described component is implemented. See [business requirements](../requirements/business-requirements.md) and the root [Readme.md](../../Readme.md).

## 2. System boundary

DEPOLEDGER coordinates deposit-related workflows across parties and systems. Its key boundaries are:

- **Channels and APIs:** capture customer and operator instructions; do not grant financial authority merely by presenting a UI action.
- **Identity and controls:** authenticate identities and evaluate roles, mandates, policy, limits, screening, and approvals.
- **Domain/application services:** implement use cases and coordinate bounded contexts.
- **Durable workflow orchestration:** persist progress across external calls, timeouts, and restarts.
- **Daml/Canton:** enforce ledger-side contractual rules, authorization, and state transitions.
- **Core banking:** authoritative for the deposit records assigned to it by the bank's operating model.
- **Payment infrastructure:** authoritative source of external payment execution status/evidence as applicable.
- **General ledger:** authoritative accounting record for designated postings.
- **Reconciliation:** independently compare records and route differences; it does not silently rewrite authorities.

## 3. Logical flow

An instruction enters through an API or authorized operator channel, passes identity and policy checks, and becomes a durable workflow. The workflow coordinates approvals, external checks or reservations, authorized ledger operations, banking/payment actions, and reconciliation. Each system commits independently; the workflow records each outcome and recovers using idempotent commands and explicit exception handling.

## 4. Architectural principles

1. Contractual and ledger invariants are enforced on-ledger where appropriate.
2. External system operations are not represented as atomic with a Daml transaction.
3. Domain code depends on ports/interfaces rather than technology-specific adapters.
4. Initiation, approval, execution, and review are separated by policy.
5. Every operation is attributable, idempotent, recoverable, and reconcilable.
6. Contract visibility, application authorization, and infrastructure access are distinct controls.
7. Product rules are versioned; existing agreements retain their applicable terms.

## 5. Layers and components

| Layer | Responsibility | Examples |
|---|---|---|
| Channels | Collect instructions and present status | Corporate portal, operations console, APIs |
| Identity/control | Authentication and transaction authorization | OIDC/MFA, RBAC/ABAC, delegated authority, approvals |
| Domain services | Product and lifecycle policy | Origination, issuance, transfers, redemption, servicing |
| Workflow execution | Durable multi-system coordination | Orchestrator, inbox/outbox, scheduler, recovery queues |
| Adapters | Vendor/protocol communication | Canton, CBS, GL, payments, compliance, treasury |
| Ledger | Contractual state transitions | Daml package and Canton network |
| External authorities | Maintain assigned bank/payment/accounting records | CBS, payment rail, GL, screening systems |
| Evidence/operations | Monitoring, reconciliation, audit, exception management | Logs, metrics, reports, case queues |

## 6. Hexagonal architecture rules

- Inbound adapters invoke application use cases.
- Use cases own orchestration decisions and depend on domain abstractions.
- Domain invariants do not depend on HTTP, database, broker, or Canton SDK details.
- Outbound ports define required capabilities; adapters translate protocols and errors.
- Integration errors map to explicit workflow states, not generic success/failure assumptions.
- Authorization must be validated at the execution boundary, not inferred from UI state.

## 7. Deployment and environment boundaries

Development, test, staging, and production require segregated configuration and credentials. Production signing authority must be restricted to approved services/operators and institution-approved custody controls. The selected Canton topology, participant operations, backup, recovery, and network governance require their own deployment design.

## 8. Technology choices and constraints

The project uses Daml and Canton concepts, but exact SDK/DPM/Canton versions and runtime commands must match repository configuration. Avoid mixing incompatible toolchains. Backend, workflow engine, database, message broker, identity provider, and HSM remain implementation decisions subject to institutional standards.

## 9. Architecture decisions to record

Create ADRs for authoritative source per field; ledger-vs-banking operation ordering; reservation and duplicate-prevention strategy; identity-to-party mapping; stakeholder visibility; approval binding; status semantics; integration delivery guarantees; reconciliation scope/frequency; recovery/compensation; and production deployment/topology.