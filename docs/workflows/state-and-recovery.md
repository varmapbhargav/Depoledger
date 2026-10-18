# Workflow State, Failure, and Recovery

## 1. Principles

Financial workflows span independent systems and cannot assume a distributed atomic transaction. Persist every step durably. Model uncertainty explicitly. A timeout is not a failure result. Retries must be idempotent and based on the same operation identity. Compensation is a new authorized financial action, not erasure of history.

## 2. Separate state dimensions

| Dimension | Example values |
|---|---|
| Deposit lifecycle | `PENDING`, `ACTIVE`, `MATURED`, `CLOSED` |
| Token lifecycle | `NOT_ISSUED`, `ACTIVE`, `FROZEN`, `REDEEMED` |
| Transaction | `RECEIVED`, `VALIDATED`, `PENDING_APPROVAL`, `AUTHORIZED`, `EXECUTION_PENDING`, `AWAITING_CONFIRMATION`, `RECONCILIATION_PENDING`, `COMPLETED`, `REJECTED`, `FAILED`, `RECOVERY_REQUIRED`, `RECONCILIATION_EXCEPTION` |
| Settlement | `NOT_REQUIRED`, `PENDING`, `CONFIRMED`, `FAILED`, `UNKNOWN` |
| Compliance | `PENDING`, `CLEARED`, `REVIEW_REQUIRED`, `RESTRICTED` |
| Reconciliation | `UNRECONCILED`, `MATCHED`, `EXCEPTION`, `RESOLVED` |

A single operation may be ledger-executed while settlement is unknown and reconciliation is pending. Do not flatten these states into a misleading `success` flag.

The transaction values above are the shared baseline. Workflows may define additional operation-specific states—for example, issuance may distinguish `FUNDING_UNCONFIRMED` and `ISSUED_UNRECONCILED`—but those extensions must not collapse execution, settlement, compliance, or reconciliation into one status.

## 3. Baseline lifecycle

`RECEIVED → VALIDATED → PENDING_APPROVAL (if required) → AUTHORIZED → EXECUTION_PENDING → AWAITING_CONFIRMATION (if applicable) → RECONCILIATION_PENDING → COMPLETED`.

At any step, validation or approval may reject the instruction. `FAILED` should be used only when failure is established; a timeout or uncertain external result remains pending/unknown until investigated. Execution/confirmation failures may transition to `RECOVERY_REQUIRED`; unresolved evidence may transition to `RECONCILIATION_EXCEPTION`. Define transitions and allowed resume points per operation type. Operation-specific workflows may add states but must preserve these distinctions.

## 4. Recovery procedure

1. Freeze automated retries for ambiguous irreversible operations.
2. Load durable workflow state, operation ID, prior request digest, and all external references.
3. Query the system authoritative for the uncertain outcome using the original identity/reference.
4. Classify as succeeded, definitively failed, or still unknown.
5. If succeeded, resume downstream work without repeating the external effect.
6. If definitively failed, retry only when policy permits and the operation is demonstrably safe; otherwise route to controlled recovery.
7. If still unknown, escalate to the manual investigation queue and maintain an unresolved status.
8. Apply compensation only with explicit authority and preserve both original and compensating evidence.
9. Reconcile after recovery and capture reviewer, rationale, timestamps, and evidence.

## 5. Durable integration patterns

Use a transactional outbox/inbox where appropriate, stable deduplication keys, schema versioning, message validation, bounded retries with backoff, dead-letter/exception handling, and replay controls. Record delivery and business-processing outcomes separately. Out-of-order messages must be checked against workflow version/state before applying.

## 6. Exception case requirements

Each case needs unique ID, related operation and contract references, severity, creation/update times, accountable owner, source evidence, remediation deadline, escalation policy, restricted access, resolution rationale, approval where required, and immutable history.

## 7. Operational measures

Monitor age/count of pending approvals, unknown settlement outcomes, recovery attempts, dead-letter messages, unreconciled operations, and exception cases. Thresholds and service objectives must be set by accountable owners before production.