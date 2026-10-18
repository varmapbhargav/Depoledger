# Business Requirements

## 1. Purpose and status

DEPOLEDGER is a proposed controlled platform for issuing, administering, transferring where permitted, redeeming, and reconciling tokenised representations of bank deposit liabilities. This document translates the project overview into business outcomes and controls. It does not define a legal conclusion or represent completed implementation.

## 2. Business objectives

- Preserve the issuing bank's contractual deposit obligations when a deposit is digitally represented.
- Ensure issuance is supported by an authorized deposit entitlement and cannot be duplicated.
- Support approved deposit products and controlled lifecycle operations.
- Permit transfers only when the product terms, holder authority, counterparty eligibility, and applicable policy allow them.
- Coordinate redemption and withdrawal with authoritative banking and payment systems.
- Separate initiation, approval, execution, and independent review where required.
- Recover safely from timeouts, duplicate events, outages, and partial completion.
- Maintain attributable evidence for operations, accounting, compliance, and audit.
- Detect and resolve differences across deposit, ledger, settlement, and accounting records.

## 3. Financial integrity principle

For a fully tokenised product, the intended relationship is that the tokenised entitlement corresponds to the associated deposit entitlement. The implementation must also account for partial tokenisation, accrued interest or contractual profit, fees, holds, and non-tokenised balances. Nominal equality alone is not proof of enforceable rights, available liquidity, settlement finality, or correct accounting.

## 4. Scope

### Initial target scope

- A restricted, single-bank corporate demand-deposit product.
- Eligible corporate customers and authorized signatories.
- Product-governed issuance with duplicate prevention.
- Transfers only between eligible participants where contractually permitted.
- Controlled redemption/withdrawal coordination.
- Maker-checker and delegated authority controls.
- Approved compliance screening integration.
- Core banking and general-ledger reconciliation.
- Durable workflow state, audit evidence, and exception handling.

### Future capabilities requiring separate approval

Time, notice, call, wholesale, and interbank deposits; brokered deposits; structured deposits; and Islamic deposit arrangements. Each requires separate contractual terms, lifecycle rules, compliance/legal review, operational procedures, and acceptance tests. Future scope must not be interpreted as included in the first release.

### Out of scope unless separately approved

- Unrestricted or permissionless token issuance.
- Replacing the authoritative core banking system or general ledger.
- Treating ledger submission as proof of external settlement or legal finality.
- Treating a tokenised deposit as automatically equivalent to a stablecoin, cryptoasset, or central bank digital currency.
- Production deployment before required legal, regulatory, security, resilience, and operational approvals.

## 5. Business actors and accountability

| Actor | Business responsibility |
|---|---|
| Depositor / authorized signatory | Submits instructions within account mandate and accepts applicable terms. |
| Relationship manager | Initiates eligible customer deposit requests. |
| Operations maker | Creates controlled operational instructions. |
| Operations checker / delegated approver | Independently approves designated instructions. |
| Treasury dealer / manager | Proposes and approves funding transactions within authority. |
| Compliance officer / fraud analyst | Reviews screening results, restrictions, and suspicious activity. |
| Finance officer | Reconciles accounting evidence and manages financial exceptions. |
| Product governance authority | Approves product versions and activation. |
| Legal / Sharia reviewer | Reviews applicable contractual and product structures. |
| Information security | Governs access, keys, security controls, and incidents. |
| Internal auditor | Independently evaluates evidence and control effectiveness. |
| System administrator | Operates infrastructure; has no implied financial authority. |
| Core banking / GL / payment provider | Supplies authoritative records or execution evidence for its assigned domain. |

Human identity, application role, Canton party, Daml stakeholder, and service identity are distinct and must be mapped explicitly.

## 6. Business rules

1. Every financial instruction has a unique, stable operation identity.
2. No representation is issued without an approved product version, eligible party, valid instruction, required approvals, and authoritative evidence of the supporting deposit entitlement.
3. Transfer and redemption amounts must respect available amounts, holds, product rules, and currency/precision rules.
4. Approval is bound to the exact material transaction details; a material change invalidates prior approval.
5. Execution, settlement, deposit lifecycle, and reconciliation statuses remain distinct.
6. Unknown external outcomes are investigated before retrying an irreversible operation.
7. A ledger archive or compensating action does not erase historical evidence.
8. Reconciliation differences create owned, traceable exceptions; they are not silently rewritten.
9. Existing agreements remain governed by the approved terms applicable at origination unless a valid contractual amendment is executed.
10. Emergency actions are limited, attributable, reviewed, and documented.

## 7. Business outcomes and acceptance

| Outcome | Minimum acceptance evidence |
|---|---|
| Authorized issuance | Contract/workflow tests show valid issuance succeeds and unauthorized or duplicate issuance is prevented. |
| Controlled transfer | Tests demonstrate holder authority, eligibility, limits, and required approvals are enforced. |
| Safe redemption | Recovery tests show uncertain payout outcomes are resolved before retry and duplicate payout is prevented. |
| Accurate records | Reconciliation detects mismatches among designated authoritative systems and records a case. |
| Operational accountability | Each exception has an owner, severity, evidence, deadline, escalation, and resolution record. |
| Governed products | Version changes are approved, tested, and do not silently alter existing agreements. |
| Production readiness | Legal, regulatory, security, privacy, resilience, operational, and business sign-offs are evidenced. |

## 8. Decisions requiring business approval

Before implementation of a real product, record decisions for: authoritative system per financial field; legal nature of the tokenised entitlement; exact issuance ordering; reservation/posting model; transferability and finality; redemption and payment behavior; approval thresholds; limits and segregation rules; data visibility; retention; exception ownership; and product-specific interest/maturity conventions.