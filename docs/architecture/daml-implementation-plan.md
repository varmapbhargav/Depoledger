# Daml Implementation Plan

## Status and assurance boundary

This is the dependency-ordered implementation plan for the current Daml workspace. The source repository is a prototype, not a production-ready banking platform. Code that passes package and script tests is only one part of assurance; it does not establish legal enforceability, operational suitability, regulatory approval, production security, or bank-grade readiness. The README's production-readiness criteria remain release gates.

The workspace is a multi-package build declared in `multi-package.yaml`, which pins Daml SDK 3.5.11 and lists the `main` (package `depoledger-main`) and `test` (package `depoledger-test`, which data-depends on the built `main` DAR) packages. The workstation toolchain must provide that exact compatible SDK (or the project owner must approve a version change) before results can be considered authoritative. The local Daml Assistant may be legacy; do not silently change the package version to make a local build pass.

Record every module and requirement against the status vocabulary in [docs/README.md](../README.md) (`Proposed`, `Approved`, `Implemented`, `Verified`, `Production-approved`). A requirement can be implemented without being production-approved, and nothing below is production-approved.

## Current implementation status

The workspace already contains working modules; the remaining phases are proposed.

- Implemented: `Common.Types`, `Governance.Approval`, `Products.DepositProduct`, `Deposits.DepositAgreement`, `Lifecycle.StateRules`, and `Internal.Authorization`.
- Scaffold only: `main/daml/Main.daml` and `test/daml/Test.daml` still contain the default Daml template sample (`Asset`/`Give`). They are not the package facade or real deposit tests and must be replaced, not extended, by phase 16.
- Not implemented: phases 4–8 and 10–14.
- No module is recorded as Verified against the pinned SDK here. Build and test with the repository-pinned SDK (see the root [Readme.md](../../Readme.md)) and record the result before marking anything Verified.

## Scope and dependencies

Implement one bounded module per concern and avoid import cycles. Cross-system orchestration, external calls, schedulers, durable retries, and payment/core banking actions remain outside Daml. Daml templates hold contractual facts and enforce ledger-side transitions only.

Module names follow the README repository layout and the bounded contexts in [domain-and-daml-design.md](domain-and-daml-design.md); the on-ledger position is the `DepositPosition` (the README's "eligible digital representation"), not a separate tokenization context. Requirement IDs are the stable references from [functional-requirements.md](../requirements/functional-requirements.md) and [non-functional-requirements.md](../requirements/non-functional-requirements.md).

| Phase | Module | Responsibility | Depends on | Status | Requirements |
|---|---|---|---|---|---|
| 0 | `Common.Types` | Currency-aware amounts, status enums, shared identifiers/evidence references and validation helpers | None | Implemented | FR-010, FR-011, NFR-001, NFR-002 |
| 1 | `Governance.Approval` | Single-use approval evidence bound to an operation/action/digest/product version and separated maker/checker | Common | Implemented | FR-012, FR-013, FR-014, FR-015 |
| 2 | `Products.DepositProduct` | Approved immutable product terms/version and activation/retirement governance | Common | Implemented | FR-004, FR-005 |
| 3 | `Deposits.DepositAgreement` | Agreed deposit terms, minimized external account reference, lifecycle state and product reference | Common, Products | Implemented | FR-003, FR-004, FR-022, NFR-001 |
| 4 | `Deposits.DepositPosition` | Bank/holder position representation and offer/accept transfer with full authorization | Common, Deposits, Products, Governance | Proposed | FR-030, FR-031, FR-032, FR-033 |
| 5 | `Issuance.IssuanceRequest` | Single-use issuance request and approved position creation against active agreement/product | Common, Deposits, Products, Governance, DepositPosition | Proposed | FR-020–FR-024 |
| 6 | `Transfers.TransferRequest` | Transfer case/operation evidence and status around position offer/acceptance where needed | Common, DepositPosition, Governance | Proposed | FR-030–FR-034 |
| 7 | `Redemption.RedemptionRequest` | Single-use request, partial/full retirement, and status distinct from payout settlement | Common, Deposits, DepositPosition, Governance | Proposed | FR-040–FR-044 |
| 8 | `Settlement.SettlementEvidence` | External instruction/confirmation references and settlement state, never external execution | Common | Proposed | FR-034, FR-044, FR-060, FR-061, NFR-008 |
| 9 | `Lifecycle.StateRules` | Controlled agreement state transitions and restrictions | Common, Deposits, Governance | Implemented (demand-deposit transitions only) | FR-052, FR-072, FR-073 |
| 10 | `Maturity.MaturityInstruction` | Versioned maturity/accrual/renewal instruction evidence; calculation remains off-ledger | Common, Deposits, Products, Governance | Proposed | FR-050–FR-054 |
| 11 | `Treasury.FundingProposal` | Governed funding proposal, terms digest, approval reference and settlement reference | Common, Governance, Settlement | Proposed | FR-011–FR-013, FR-060 |
| 12 | `Reconciliation.ReconciliationCase` | Evidence-linked discrepancy case, owner, severity, and reviewed resolution | Common | Proposed | FR-063–FR-065 |
| 13 | `Audit.AuditEvidence` | Append-only evidence references for decisions and external events | Common | Proposed | FR-070, FR-071, NFR-007 |
| 14 | `Operations.ControlDirective` | Scoped pause/restriction directive and review evidence; no infrastructure administration | Common, Governance | Proposed | FR-072, FR-073 |
| 15 | `Internal` | Private helpers only when needed; must not become an alternate public API | Common | Implemented (`Internal.Authorization`) | NFR-010, NFR-002 |
| 16 | `Main` and test package | Minimal package facade plus positive/negative Daml Script scenarios | All supported public modules | Scaffold only | FR-071, NFR-011 |

## Decisions that gate phases

The phase order assumes decisions that are still open in the [ADR backlog](decisions/README.md). Do not treat a requirement ID, workflow diagram, or option list as an approved design. At minimum:

- Identity, mandate, and party-to-controller mapping (ADR 3) before phases 4–7.
- Issuance/redemption ordering, reservation, and recovery model (ADR 2) before phases 5 and 7.
- Contract stakeholders and visibility (ADR 4) across every template.
- Product/version governance and compatibility (ADR 5) before phase 10.
- Integration delivery, idempotency, and status/finality semantics (ADR 6) before phase 8.
- Reconciliation keys, tolerances, and exception ownership (ADR 8) before phase 12.

## Security and data rules

1. Every party field is data, not authority. Choices explicitly name controllers.
2. A contract creation is authorized only when all declared signatories are authorized for the command. Creation paths must be designed and tested with this rule in mind.
3. Approval records bind to a canonical operation ID, action, digest, product version, initiator and checker. Canonical digest construction and cryptographic verification remain application/service responsibilities; Daml compares the exact supplied value.
4. Shared operation identifiers alone do not prevent duplicate workflows across templates. Use consumed request contracts and explicitly documented ledger keys where supported and approved; test concurrency and replay on the target Canton version.
5. Amounts use exact Daml numeric semantics, explicit currency, positive/range checks, and product precision rules. Avoid `Double` and implicit FX.
6. Keep PII and raw account details off-ledger. Store only bank-approved pseudonymous references/digests with governed lookup and retention.
7. No template claims to prove external funding, payment settlement, accounting posting, legal finality, screening completion, or identity verification without trusted and governed evidence.
8. Avoid generic “admin” choices that bypass normal authorization. Emergency authority must be narrowly scoped, dual-controlled where policy requires, and audited.
9. Keep status dimensions separate: deposit lifecycle, token lifecycle, transaction lifecycle, settlement, compliance, and reconciliation must not be collapsed into one status. Follow the shared baseline in [state-and-recovery.md](../workflows/state-and-recovery.md); operation-specific extensions may add states but must not merge execution, settlement, compliance, or reconciliation.

## Implementation and verification gates

For each phase:

1. Implement the smallest module and document authority, stakeholders, state transitions, and evidence.
2. Build with the repository-pinned SDK and treat warnings as review items.
3. Add positive, unauthorized, malformed-input, replay, concurrency, and visibility tests appropriate to the module.
4. Run the package test suite and inspect generated DAR metadata.
5. Review privacy, authorization, upgrade compatibility, and package dependencies.
6. Mark only what is actually implemented and verified in requirement tracking.

These gates are the Daml-level subset of the testing layers and release gates in [testing-and-release.md](../operations/testing-and-release.md); a phase is not complete until its module builds, its tests pass, and any affected requirement ID is updated with evidence.

Before production, additionally require approved legal/product terms, institution-specific regulatory and accounting review, Canton topology and version compatibility, HSM/signing controls, tested core banking/GL/payment/compliance adapters, durable orchestration and recovery, independent reconciliation, privacy/security assessments, operational runbooks, resilience exercises, service objectives, and accountable sign-offs. These are the release gates in [testing-and-release.md](../operations/testing-and-release.md) and the checklist in the root [Readme.md](../../Readme.md).

## Explicitly deferred

- Live core banking, GL, payment, compliance, and treasury integrations.
- Workflow orchestration, durable inbox/outbox, queues, schedulers, and retries.
- Cryptographic digest generation/verification and identity/mandate resolution.
- Interest calculations, FX, pricing, liquidity, sanctions, and legal determinations.
- Canton production topology, participant operations, HSM configuration, and deployment.

These cannot be supplied by Daml contract files alone.
