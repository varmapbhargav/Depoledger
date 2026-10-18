# Authorization, Security, and Privacy

## 1. Security model

No single role or technology provides complete authority. Enforce controls across enterprise identity, customer mandate, application policy, workflow approval, Daml controller/signatory rules, Canton participant operations, and key custody. Document which boundary enforces each control.

## 2. Identity and access

- Authenticate human users and services using institution-approved mechanisms; require MFA and appropriate session/device controls for human access.
- Resolve identities to roles, mandates, and Canton parties through governed mappings; validate mappings and revocation.
- Use least privilege and separate maker, checker, execution, administration, security, and audit duties as required.
- Infrastructure administration must not imply transaction or signing authority.
- Review service credentials and access periodically; revoke promptly on role, employment, or incident changes.

## 3. Transaction authorization

For each operation validate actor, account mandate, product/transaction policy, limits, recipient/counterparty, applicable compliance status, required approvals, current contract state, and Daml controller authority. Approval must be independent where mandated and bound to exact transaction facts and a policy version. Revalidate mutable conditions at execution.

## 4. Key and signing controls

Use institution-approved HSM or equivalent custody and Canton signing configuration. Restrict signing capabilities to approved workloads and transaction scopes. Define key generation, access, backup where supported, rotation, revocation, compromise response, and recovery procedures. Test emergency containment without destroying auditability. Never store production secrets in source control or ordinary application logs.

## 5. Threats and controls

| Threat | Control themes |
|---|---|
| Unauthorized issuance | Product/authority checks, authoritative entitlement evidence, approval, Daml authorization, reconciliation |
| Double redemption/payment | Reservation, concurrency control, stable IDs, provider status query, deduplication |
| Compromised key/service | HSM, restricted signer, workload identity, monitoring, rotation/revocation |
| Insider misuse | Segregation, independent approval, privileged activity monitoring, immutable evidence |
| Fraudulent recipient | Recipient eligibility, screening, transaction monitoring, limits |
| Replay or duplicate message | Stable IDs, durable inbox, request digest, state/version checks |
| Malicious product configuration | Immutable versions, governance approvals, review and tests before activation |
| Integration compromise | Mutual authentication, least-privilege credentials, schema/input validation |
| Privacy disclosure | Data minimization, stakeholder analysis, access reviews, encryption |
| Conflicting settlement outcome | Authority mapping, explicit unknown state, reconciliation and escalation |

## 6. Daml/Canton privacy

- Define signatories and observers only for justified business need.
- Test which parties see contract data and transaction details for representative flows.
- Do not assume application RBAC restricts ledger visibility.
- Minimize personal data and use controlled references where legally and operationally appropriate.
- Consider participant topology, transaction submission, divulgence, and authorized audit access in privacy reviews.

## 7. Data protection

Classify data; encrypt sensitive data in transit and at rest; restrict logs, metrics, and traces; define retention, deletion, legal hold, and evidence-pointer durability; govern cross-bank disclosure; and document regulator/auditor access. A permissioned ledger is not automatically private.

## 8. Assurance and incident response

Perform threat modeling, code and dependency review, authorization/privacy testing, vulnerability assessment, key-custody review, and independent security assessment. Incident runbooks must cover compromised credentials/keys, unauthorized transaction, privacy incident, participant outage, and external provider compromise. Preserve forensic evidence and follow institutional notification/escalation obligations.