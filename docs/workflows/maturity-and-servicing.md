# Workflow: Deposit Servicing and Maturity

## 1. Scope

Applies to future time, notice, call, structured, and other products only after product-specific legal/business approval. Demand-deposit servicing may have a simpler lifecycle. No generic interest or profit treatment is assumed.

## 2. Governing terms

At origination retain the executed contractual terms and product version. Calculation policy must define rate/profit treatment, day-count convention, compounding, business calendar, timezone/cut-off, observation source, precision, rounding, fees, early redemption, notice revocation, and dispute handling where applicable. Methodology versions and input evidence must be reproducible.

## 3. Maturity sequence

1. Detect due event using a stable deposit/product identity and schedule reference.
2. Load applicable immutable terms and customer instructions.
3. Validate calendar, time zone, holds, restrictions, screening state, and whether review is required.
4. Calculate or obtain the authorized contractual entitlement and retain inputs/version/evidence.
5. Route exceptions or approval-required outcomes to authorized review.
6. Select a permitted outcome: payout/redemption, renewal/rollover, or other contractually supported action.
7. Coordinate ledger, core-banking, payment, and GL operations under the approved workflow.
8. Reconcile final records and close only when completion criteria are met.

## 4. Duplicate and exception controls

- Use a unique maturity/accrual/rollover operation ID and deduplicate repeated scheduler events.
- Never create duplicate accruals, renewals, postings, or payouts.
- Calculation disagreement, missing terms, invalid dates, failed integration, or unknown payment outcome must enter a controlled exception queue.
- A renewal creates an authorized new term/agreement or amendment; it must not silently overwrite prior terms.

## 5. Product extensions

Structured deposits require approved payoff formulas, observation sources, model validation, and dispute evidence. Islamic arrangements require product-specific legal and Sharia review of underlying transactions, profit/loss treatment, ownership, and disclosures. Brokered deposits require broker authorization and beneficial-owner provenance. These cannot be inferred from a conventional deposit model.

## 6. Acceptance tests

Test calendar boundaries, leap dates, timezone/cut-off, rounding, product version preservation, early redemption, notice expiry/revocation, repeated maturity events, failed payout, disputed calculation, rollover approval, and reconciliation.