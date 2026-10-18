# Workflow: Treasury and Interbank Funding

## 1. Scope

Target for future institutional funding and interbank deposit workflows. Requires separate approved products, counterparty agreements, legal review, network participation, settlement model, and risk controls.

## 2. Controls before commitment

Validate dealer mandate, counterparty eligibility, exposure and concentration limits, currency, tenor, rate/terms, maturity, liquidity impact, product version, screening, bilateral confirmation requirements, independent treasury approval, and operation identity. Approval must bind to exact transaction terms.

## 3. Reference sequence

1. Dealer proposes transaction with complete terms and unique operation ID.
2. Risk service evaluates counterparty, limits, concentration, and policy.
3. Independent manager approves or rejects the exact proposal.
4. Counterparty confirms terms through the approved channel.
5. Workflow coordinates settlement instruction and captures status/evidence.
6. Authorized Daml/Canton state is recorded at the legally and operationally approved point in the process.
7. Finance/ALM receives references and records or verifies accounting outcomes.
8. Reconciliation compares agreement, ledger, payment, and GL evidence; maturity monitoring is scheduled.

Whether the ledger state is recorded before, during, or after external settlement is an explicit product/legal decision. Do not interpret this reference flow as a mandated ordering or atomic transaction.

## 4. Failure cases

Handle limit changes, counterparty rejection, mismatched confirmation, settlement timeout/unknown outcome, ledger submission failure, accounting rejection, and maturity failure through durable workflow states and controlled recovery. Query authoritative systems before retrying irreversible settlement.

## 5. Acceptance tests

Test maker-checker separation, exact-term approval, limit boundary and concurrent exposure, counterparty mismatch, duplicate proposal, settlement unknown, recovery without duplicate payment, ledger/settlement ordering, accounting evidence, and maturity reconciliation.