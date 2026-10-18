# Workflow: Redemption and Withdrawal

## 1. Purpose

Coordinate retirement or adjustment of a tokenised position with the deposit obligation, core-banking action, optional payout, accounting, and reconciliation. Redemption may not always require an external payment; product terms determine the route.

## 2. Preconditions

- Requester has current holder and account authority.
- Product permits redemption at this time; notice, maturity, early-redemption, and fee rules are satisfied.
- No restriction, hold, or competing reservation prevents the amount.
- Core banking confirms the authoritative entitlement and approved available amount.
- Required compliance checks and approvals have passed.
- The operation has a stable unique redemption identity.

## 3. Reference sequence

1. Persist request; validate identity, authority, product rules, and operation ID.
2. Query authoritative entitlement and relevant holds.
3. Reserve or lock the eligible amount when required by the approved model.
4. Execute the authorized ledger redemption transition.
5. Submit/confirm the applicable core-banking posting or entitlement update.
6. Initiate payout only where required and authorized; capture provider reference and status.
7. Record applicable GL entries or journal references.
8. Reconcile deposit, token, payment, and accounting outcomes.
9. Close only when defined completion criteria are met; unresolved components remain visible as pending or exception states.

The precise ordering must be selected and tested for the bank's capabilities and legal model. There is no cross-system atomic commit.

## 4. Unknown outcomes and safety

A payment timeout is not proof of failure. Query by the original payment reference before any retry. Prevent concurrent redemption using reservations/locking or an equivalent control. Never blindly retry an irreversible payout. Do not restore a token position or alter a deposit record through a silent correction; use approved compensating actions with full audit evidence.

## 5. Completion criteria

Define per product whether completion requires ledger retirement, core-banking posting, payment confirmation, GL evidence, and reconciliation match. Acknowledgement and settlement finality must be distinguished.

## 6. Acceptance tests

Cover partial redemption, holds, maturity/notice restrictions, concurrent requests, duplicate instruction, CBS timeout, ledger timeout, payment unknown/confirmed/failed, accounting mismatch, safe recovery, and required approval.