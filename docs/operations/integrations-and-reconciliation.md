# Integrations and Reconciliation

## 1. Integration principles

1. Document the authoritative source for every financial field, business event, and status.
2. Treat each external call as independently committed; no implicit atomicity with Canton.
3. Use versioned APIs/schemas, stable operation and correlation IDs, and authenticated least-privilege service identities.
4. Expect duplicate, delayed, missing, and out-of-order messages.
5. Validate payload schema, business references, currency/amount, timestamps, and state transitions.
6. Use transactional outbox/inbox and durable deduplication where appropriate.
7. Query actual outcome before retrying an irreversible action.
8. Preserve raw/normalized evidence and processing results subject to data minimization and retention policy.

## 2. Adapter boundaries

Separate adapters for core banking, GL, payments/RTGS, compliance, treasury/ALM, and Canton. Each adapter should map external schemas to domain types, translate provider statuses without losing meaning, handle authentication and timeouts, expose correlation references, and report health without owning business policy.

## 3. Status mapping

For every provider define success, rejected, pending, failed, cancelled, expired, and unknown semantics. Record provider timestamps and query capability. Do not map an acknowledgement to settlement finality unless the provider and legal model define it that way.

## 4. Reconciliation scope

Compare designated records from:

- Core banking deposit entitlement, balances, holds, and postings.
- Canton contracts, positions, transaction evidence, and applicable package/product version.
- Payment/settlement instructions and confirmations.
- General-ledger journals and relevant suspense accounts.

Reconcile per operation and through scheduled/end-of-day controls where required. Define matching keys, extraction cut-offs, timing windows, currency precision, tolerances, partial tokenisation, accrued amounts, and expected timing differences per product.

## 5. Reconciliation process

1. Collect versioned, timestamped source extracts/evidence.
2. Normalize without altering source facts.
3. Match by approved operation/account/contract/transaction/journal references.
4. Apply product-specific invariants and tolerances.
5. Persist match evidence or open an exception case.
6. Assign an accountable owner and severity; track deadline and escalation.
7. Investigate source-of-truth and workflow history before correction.
8. Apply only approved, attributable correction or recovery.
9. Re-run comparison and preserve before/after evidence and sign-off.

## 6. Exception categories

Unbacked representation; missing/stale representation; amount/entitlement mismatch; duplicate or absent journal; missing/contradictory settlement evidence; duplicate interest/maturity/redemption; restricted-account activity; delayed integration or stale status; and inconsistent source snapshots.

## 7. Operational controls

Monitor adapter availability, latency, rejection rate, backlog, duplicate detection, unknown outcomes, schema failures, reconciliation age/mismatch count, and exception SLA. Alert thresholds and processing objectives require service-owner approval. Runbooks must identify primary/secondary support, provider escalation, safe restart/replay, and no-blind-retry steps.