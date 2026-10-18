# Domain and Daml Design

## 1. Bounded contexts

| Context | Owns | Does not own |
|---|---|---|
| Customer eligibility | Eligibility decisions and evidence references | Authoritative customer identity records |
| Product governance | Approved, versioned product rules | Unilateral changes to existing agreements |
| Deposit origination | Application and agreed terms | External core banking balances |
| Deposit lifecycle | Contract state and allowed lifecycle changes | External payment finality |
| Token issuance | Issuance request and authorized ledger instruction | Unrestricted creation of deposit liabilities |
| Transfer management | Transfer instruction and policy outcome | All banking/payment/accounting state |
| Redemption | Redemption workflow and lifecycle coordination | Guaranteed payout completion |
| Interest and maturity | Calculation inputs/rules and instructions | Unapproved pricing decisions |
| Treasury and funding | Funding proposal, counterparty/limit evidence | Customer identity master data |
| Settlement | Payment instructions and settlement evidence | Unilateral change to deposit obligations |
| Accounting | Posting requests and journal references | Ledger consensus |
| Reconciliation | Cross-system comparisons and cases | Silent rewriting of authority records |
| Audit and governance | Evidence and policy versions | Transaction initiation authority |

## 2. Daml conceptual model

The README's conceptual entities may map to templates, data types, or off-ledger records; do not create a contract for every noun by default.

- **Product definition/version:** approved parameters and immutable version reference.
- **Deposit agreement:** contractual relationship, parties, terms, and lifecycle.
- **Deposit position:** eligible digital representation and its current holder/amount model.
- **Issuance, transfer, redemption requests:** workflow evidence where on-ledger representation is justified; otherwise durable off-ledger workflow with ledger references.
- **Approval record:** exact instruction binding, approver, policy/version, decision, and timestamp reference.
- **Settlement/reconciliation reference:** minimized external identifiers and evidence links.

## 3. Contract review checklist

For each template and choice document its purpose, lifecycle, signatories, observers, controllers, preconditions, outputs, consumed contracts, visibility, archival/replacement behavior, and off-ledger references. Test positive and negative paths, including authorization and privacy.

## 4. Daml/Canton constraints

- Contract creation requires the declared signatories.
- Choice authority is determined by the controller expression; a party stored in a data field is not automatically authorized.
- Contract visibility follows Daml/Canton stakeholder and transaction semantics, not application RBAC alone.
- Archival does not erase historical ledger transactions.
- A ledger transaction cannot atomically commit a CBS, GL, or external payment transaction.
- Canton privacy and participant governance must be designed explicitly; permissioned participation does not imply universal secrecy.

## 5. Authorization composition

A complete decision may combine authenticated enterprise identity, customer mandate/delegation, application policy and limits, required independent approval, Canton party mapping, and Daml controller authority. Record policy versions and the exact transaction facts. Revalidate at execution time where state may have changed.

## 6. Versioning and evolution

New product versions should be approved and activated explicitly. Existing agreements retain their original terms unless a permitted amendment is agreed and recorded. Template evolution, package dependencies, upgrades, and data migration must follow the supported Daml/Canton toolchain and be tested against existing contracts.

## 7. Testing expectations

Tests must cover contract creation, invalid preconditions, unauthorized choices, amount/currency validation, product policy, approval conditions, visibility, transfers, redemption, lifecycle, and archival/replacement. See [testing and release assurance](../operations/testing-and-release.md).