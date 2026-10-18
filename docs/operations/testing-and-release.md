# Testing and Release Assurance

## 1. Testing layers

- Daml unit and contract tests for creation, choices, controllers, preconditions, stakeholders, and lifecycle.
- Domain invariant tests for amounts, currencies, product rules, holds, and status transitions.
- Authorization and privacy tests for roles, mandate mapping, maker-checker independence, and visibility.
- Workflow scenario tests for issuance, transfer, redemption, servicing, and treasury.
- Adapter contract/integration tests against approved test environments and schema versions.
- Failure and recovery tests for duplicate, delayed, out-of-order, unknown, and failed outcomes.
- Reconciliation tests with matched, mismatched, partial, stale, and corrected records.
- Security, performance, resilience, business acceptance, and operational exercises.

## 2. Required failure tests

Test CBS timeout and duplicate response; ledger timeout and uncertain submission; payment unknown outcome; duplicate/out-of-order event; restart at every workflow boundary; participant/provider unavailability; failed GL posting; stale approval or changed instruction; malformed external message; recovery after partial execution; end-of-day mismatch; and backup/restore.

## 3. Release gates

A release requires reviewed requirements and architecture decisions; build and test evidence; Daml/Canton compatibility verification; dependency and artifact provenance; security/privacy assessment appropriate to change; migration and rollback/recovery plan; staging integration results; operations runbooks and dashboards; business/control owner approval; and release authorization under institutional governance.

## 4. Production readiness evidence

- Every issuance maps to an authorized entitlement.
- Duplicate issuance/redemption is prevented.
- Ledger authorization and visibility tests pass.
- Financial amounts, holds, and rounding are verified.
- External statuses and finality semantics are documented.
- Core banking, ledger, settlement, and GL reconciliation is operational.
- Unknown outcomes and recovery were exercised.
- Key custody, access controls, incident response, and disaster recovery are reviewed.
- Legal, regulatory, product, customer disclosure, data retention, and supervisory decisions are documented.
- Accountable sign-offs and residual risks are recorded.

## 5. Change management

Changes to product terms, templates, workflow state, schemas, approval policy, identity mapping, or authoritative-source rules may affect existing operations. Assess compatibility, migrate deliberately, maintain package/schema versions, test active workflows and contracts, and preserve a rollback/recovery route. Do not alter existing agreements silently.