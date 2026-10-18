# Data and Financial Model

## 1. Design objective

Model the contractual deposit entitlement and its digital representation without confusing ledger state with the bank's authoritative account records, payment state, or accounting state.

## 2. Logical relationships

A bank issues deposit agreements to eligible depositors under approved product versions. Agreements may support one or more positions/representations, lifecycle instructions, transfers, redemptions, settlement references, and reconciliation cases. This is a logical model, not a requirement to store every entity in one database or on-ledger.

## 3. Deposit information

A deposit record may reference:

- Issuing bank, jurisdiction, depositor, mandate, and core banking account reference.
- Product ID and immutable product version.
- Currency, principal, available amount, holds, and non-tokenised balance.
- Accrued interest or contractual profit entitlement, fees, and rounding basis.
- Origination, notice, maturity, and lifecycle dates.
- Tokenisation status and outstanding representation.
- Transfer restrictions and eligible destinations.
- Approval, compliance, issuance, redemption, settlement, and accounting references.
- Reconciliation status and evidence pointer.

Personal and sensitive information should normally remain in bank-controlled systems. Prefer minimized identifiers and evidence references in Daml contracts. Define access control and retention for every reference.

## 4. Financial invariants

The product model must explicitly define how the following reconcile: total deposit entitlement, tokenised amount, non-tokenised amount, accrued entitlement, holds, fees, and available amount. Do not assume that a simple nominal token-to-deposit equality covers all cases. Define rounding, currency precision, partial tokenisation, and timing boundaries.

## 5. Monetary representation

Use currency-aware decimal/scale semantics and approved rounding rules. Avoid binary floating-point for financial amounts. Validate currency precision and permitted amount ranges at domain and ledger boundaries. Never infer currency from an amount alone.

## 6. Authoritative source matrix

The bank must complete this matrix before production. One system may be authoritative for some fields and only a replica for others.

| Data/event | Proposed authority to confirm | DEPOLEDGER use |
|---|---|---|
| Customer identity and mandate | Bank customer/identity systems | Eligibility and authorization evidence |
| Deposit account and entitlement | Core banking, if designated | Issuance/redemption validation and reconciliation |
| Contract terms | Executed agreement/product governance | Ledger/workflow version reference |
| Token position and ledger history | Canton ledger | Contractual digital state evidence |
| Payment execution | Payment rail/provider | Settlement status/evidence |
| Journal and accounting balance | General ledger | Accounting and reconciliation evidence |
| Screening outcome | Approved compliance provider/system | Policy decision reference and status |
| Workflow progress | Durable workflow service | Coordination, recovery, operations |

## 7. Status dimensions

Represent separately: deposit lifecycle (`PENDING`, `ACTIVE`, `MATURED`, `CLOSED`); token lifecycle (`NOT_ISSUED`, `ACTIVE`, `FROZEN`, `REDEEMED`); transaction lifecycle (the canonical states and recovery outcomes in [Workflow State, Failure, and Recovery](../workflows/state-and-recovery.md), with operation-specific extensions); settlement (`NOT_REQUIRED`, `PENDING`, `CONFIRMED`, `FAILED`, `UNKNOWN`); compliance (`PENDING`, `CLEARED`, `REVIEW_REQUIRED`, `RESTRICTED`); and reconciliation (`UNRECONCILED`, `MATCHED`, `EXCEPTION`, `RESOLVED`). Valid state combinations must be defined per workflow.

## 8. Identifiers and lineage

Use stable operation IDs, contract IDs, package/product versions, correlation IDs, external reference IDs, and journal/payment references as appropriate. Keep mappings durable and access-controlled. Do not use a mutable display name as a financial identity or deduplication key.

## 9. Reconciliation keys and evidence

Define deterministic matching keys and tolerance/rounding rules by product, currency, event, and accounting model. Preserve source timestamps, extraction times, versions, and evidence provenance. Exceptions must retain the original mismatch and approved resolution; correction must not erase history.