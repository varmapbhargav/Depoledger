# Functional Requirements

## 1. Purpose

The requirements below are proposed and testable. Assign a business owner and implementation status before treating any item as approved. Requirement IDs are stable references for design reviews, tests, and change requests.

## 2. Identity, eligibility, and product governance

| ID | Requirement |
|---|---|
| FR-001 | The platform shall authenticate human and service identities using institution-approved mechanisms. |
| FR-002 | It shall resolve an authenticated actor to the applicable customer mandate, service identity, and Canton party without assuming those identities are interchangeable. |
| FR-003 | It shall reject or hold instructions when customer, account, recipient, or product eligibility cannot be established. |
| FR-004 | It shall reference an approved, immutable product version for each agreement and preserve the version applicable at origination. |
| FR-005 | Product activation and material changes shall require configured governance approval and evidence. |

## 3. Instruction and approval controls

| ID | Requirement |
|---|---|
| FR-010 | Every financial instruction shall carry a stable operation ID and correlation ID. |
| FR-011 | The platform shall validate actor authority, product policy, currency, amount, limits, holds, and required screening before execution. |
| FR-012 | When maker-checker approval is required, the initiator and approver shall be independently authorized according to policy. |
| FR-013 | Approval evidence shall bind to material transaction fields, including amount, currency, beneficiary/recipient, product version, and a transaction digest or equivalent immutable representation. |
| FR-014 | A material instruction change shall invalidate existing approval and require a new decision. |
| FR-015 | The platform shall record approval, rejection, expiry, and policy decision evidence with actor and time attribution. |

## 4. Issuance

| ID | Requirement |
|---|---|
| FR-020 | Issuance shall be supported by authoritative evidence of the deposit entitlement or approved reservation/posting model. |
| FR-021 | The platform shall prevent duplicate issuance for the same business operation and entitlement. |
| FR-022 | The ledger-side contract transition shall enforce its required signatory, controller, and contractual preconditions. |
| FR-023 | Issuance status shall distinguish approval, external funding/entitlement confirmation, ledger execution, and reconciliation. |
| FR-024 | A ledger acknowledgement alone shall not cause the workflow to report final issuance completion. |

## 5. Transfers

| ID | Requirement |
|---|---|
| FR-030 | A transfer shall validate current holder authority and sufficient available amount after holds and restrictions. |
| FR-031 | A transfer shall enforce product-specific transferability and recipient eligibility. |
| FR-032 | Compliance, fraud, limit, and approval policies shall be evaluated before the authorized transfer transition. |
| FR-033 | Duplicate and replayed transfer requests shall not create duplicate financial effects. |
| FR-034 | Ledger execution, any associated external settlement, accounting, and reconciliation shall have separate statuses. |

## 6. Redemption and withdrawal

| ID | Requirement |
|---|---|
| FR-040 | Redemption shall validate holder authority, product terms, maturity/notice rules, holds, and authoritative entitlement. |
| FR-041 | The workflow shall reserve or otherwise protect the redeeming amount when required by the approved posting model. |
| FR-042 | It shall use a unique redemption identity and prevent concurrent or repeated redemption of the same available amount. |
| FR-043 | An unknown payout outcome shall be queried/reconciled before any retry that could cause a second payment. |
| FR-044 | Redemption shall preserve ledger, core-banking, payment, and accounting references and report completion only when defined criteria are met. |

## 7. Servicing and maturity

| ID | Requirement |
|---|---|
| FR-050 | Contractual terms, calculation methodology, day-count convention, calendar, timezone, and rounding rules shall be versioned and traceable. |
| FR-051 | Maturity processing shall validate applicable instructions and calculate or obtain the approved contractual entitlement. |
| FR-052 | Payout, renewal, early redemption, and notice handling shall follow product-specific contractual rules. |
| FR-053 | Duplicate accrual, maturity, rollover, or payout events shall not create duplicate financial effects. |
| FR-054 | Calculation disputes, invalid data, and failed payouts shall enter a controlled exception process. |

## 8. External integration and reconciliation

| ID | Requirement |
|---|---|
| FR-060 | The platform shall identify the authoritative source for each material field, event, and status. |
| FR-061 | Integration messages shall be versioned, correlated, validated, and safely deduplicated. |
| FR-062 | The workflow shall tolerate duplicate, delayed, out-of-order, and missing messages. |
| FR-063 | Reconciliation shall compare designated deposit, ledger, settlement, and GL evidence at operation and scheduled batch levels as applicable. |
| FR-064 | A mismatch shall create a case with unique ID, severity, owner, evidence, deadline, escalation, and approved resolution. |
| FR-065 | Corrections shall be authorized, attributable, and auditable; silent data rewrites are prohibited. |

## 9. Audit, reporting, and administration

| ID | Requirement |
|---|---|
| FR-070 | The system shall maintain an attributable timeline linking instruction, decisions, workflow steps, ledger transactions, external references, and reconciliation outcomes. |
| FR-071 | Operational reporting shall distinguish deposit, token, transaction, settlement, compliance, and reconciliation states. |
| FR-072 | Administrative access shall not imply financial authority. |
| FR-073 | Emergency pause/restriction actions shall be scoped, authorized, logged, and subject to subsequent review. |

## 10. Minimum acceptance scenarios

1. A valid issuance completes only after all configured requirements are satisfied.
2. Repeating an issuance ID does not create a second representation.
3. An unauthorized actor or ineligible recipient cannot transfer.
4. An approval cannot be reused after changing a material transaction field.
5. A redemption timeout does not trigger blind payout retry.
6. Replayed maturity events do not duplicate interest, renewal, or payout.
7. Missing settlement confirmation leaves the operation unresolved and visible to operations.
8. A deposit/ledger/GL mismatch creates an assigned reconciliation case.
9. Product version changes do not silently modify existing agreements.
10. Unauthorized restriction release and administrative financial action are rejected.