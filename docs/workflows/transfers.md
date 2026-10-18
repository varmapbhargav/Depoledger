# Workflow: Token Transfer

## 1. Purpose and boundaries

Transfer a representation only where the applicable agreement and product permit it. Ledger execution, external payment/settlement, accounting, and reconciliation are separate outcomes. An interbank transfer requires an explicitly approved legal and settlement model.

## 2. Validation

Before execution, validate current holder and mandate, sufficient available amount after holds, product transferability, recipient eligibility, jurisdiction/counterparty restrictions, compliance and fraud screening, transaction limits, operation ID/replay status, and required approval.

## 3. Reference sequence

1. Persist instruction and stable operation/correlation IDs.
2. Authenticate initiator and resolve authority over the current position.
3. Validate amount, currency, available position, restrictions, recipient, and policy.
4. Obtain independent approval where required; bind it to exact transfer details.
5. Execute the authorized Daml/Canton choice, with controller and contract preconditions enforcing ledger invariants.
6. Record ledger transaction evidence and publish any downstream accounting/settlement event using durable delivery patterns.
7. Collect external acknowledgements and settlement confirmation where required.
8. Reconcile affected deposit, token, settlement, and GL records; report each state distinctly.

## 4. Outcomes

Possible states include rejected, pending compliance, pending approval, ledger execution pending, ledger executed, settlement pending/unknown/confirmed/failed, accounting pending, reconciliation exception, and completed. Define the exact state machine per transfer type; do not equate ledger execution with payment finality.

## 5. Recovery controls

- Deduplicate by stable operation identity.
- On timeout, query the ledger or payment system using the original reference before retry.
- Do not retry external transfers when outcome is unknown until the authoritative provider has been queried or an approved manual procedure is followed.
- Maintain the ledger result even if downstream posting fails; open recovery/reconciliation work instead of erasing history.
- Compensating transactions require an explicitly authorized policy and preserve original evidence.

## 6. Acceptance tests

Test valid transfer; insufficient available amount; held/frozen position; ineligible recipient; unauthorized holder; approval binding to amount/recipient; duplicate and out-of-order messages; ledger success with settlement failure; unknown payment outcome; and reconciliation mismatch.