# Workflow: Deposit Origination and Issuance

## 1. Purpose

Coordinate a valid deposit instruction with product, eligibility, funding/entitlement, approval, ledger, and reconciliation controls. Issuance is not complete merely because a ledger command was accepted.

## 2. Preconditions

- Customer and authorized signatory/account mandate are valid.
- Product and immutable product version are approved and active.
- Amount and currency are valid; the instruction is within limits.
- Applicable compliance screening is cleared or an authorized decision permits processing.
- Authoritative deposit entitlement or an approved reservation/posting model is confirmed.
- Required approvals are complete and bound to the exact instruction.
- A stable operation ID has been checked for prior execution.

## 3. Reference sequence

1. Receive and persist instruction and correlation identifiers.
2. Authenticate actor and resolve customer mandate and ledger party.
3. Validate product, eligibility, amount, currency, limits, and screening.
4. Obtain independent approval if policy requires it; invalidate approval if material fields change.
5. Confirm or reserve the supporting core-banking entitlement according to the approved posting model.
6. Submit authorized ledger operation with the same business operation identity/reference.
7. Persist ledger result and external references.
8. Reconcile deposit authority, ledger position, and applicable GL/settlement evidence.
9. Report completion only when defined completion conditions are met; otherwise keep a named pending or exception state.

Ordering is a bank decision. These systems do not share one atomic transaction. The selected ordering, reservations, and recovery behavior require an ADR and integration tests.

## 4. Statuses

`RECEIVED`, `VALIDATED`, `PENDING_APPROVAL`, `FUNDING_UNCONFIRMED`, `AUTHORIZED`, `ISSUANCE_PENDING`, `ISSUED_UNRECONCILED`, `ISSUED_AND_RECONCILED`, `REJECTED`, `FAILED`, `RECOVERY_REQUIRED`, `RECONCILIATION_EXCEPTION`.

Define exact transition guards and terminal/reopen behavior in implementation. A timeout with unknown ledger or CBS outcome is not a confirmed failure.

## 5. Failure and recovery

- Duplicate request: return or link to the original operation; do not create another position.
- CBS timeout: query by stable reference before retry/reservation change.
- Ledger timeout: determine actual transaction outcome before retry.
- Mismatch after ledger success: preserve the position and open an owned reconciliation exception; do not silently reverse.
- Approval or screening rejection: stop execution, record evidence, and release any reservation only under approved policy.

## 6. Acceptance tests

Valid issuance succeeds once; duplicate requests produce no second representation; unauthorized maker/checker combinations fail; altered transaction details require reapproval; unknown outcome is queried before retry; ledger success plus missing core evidence remains unresolved; mismatch creates an assigned reconciliation case.