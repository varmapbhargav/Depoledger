# Non-Functional Requirements

These are target qualities to be converted into measurable service-level objectives and institutional standards before production. No numeric availability or latency target is assumed here.

| ID | Quality | Requirement |
|---|---|---|
| NFR-001 | Financial integrity | Financial commands shall be validated against authoritative data and preserve auditable evidence. |
| NFR-002 | Idempotency | Retryable operations shall use stable identities and deduplication at every relevant boundary. |
| NFR-003 | Resilience | Workflow state shall survive service restarts; timeouts and unknown outcomes shall be explicit states. |
| NFR-004 | Recoverability | Recovery and compensation shall be policy-controlled, tested, attributable, and followed by reconciliation. |
| NFR-005 | Security | Apply least privilege, strong service authentication, separation of duties, and institution-approved key custody. |
| NFR-006 | Privacy | Minimize personal data in contracts and logs; disclose contract data only to parties with justified access. |
| NFR-007 | Auditability | A reviewer shall be able to reconstruct the complete operation and its decisions from retained evidence. |
| NFR-008 | Interoperability | External interfaces and message schemas shall be versioned and validated, with explicit status mapping. |
| NFR-009 | Observability | Correlation identifiers shall link API, workflow, ledger, core banking, payment, GL, and exception events. |
| NFR-010 | Maintainability | Domain rules shall be isolated from transport, storage, ledger SDK, and vendor-specific adapter details. |
| NFR-011 | Testability | Critical invariants, access controls, privacy, failures, and recovery paths shall have automated or documented tests. |
| NFR-012 | Data governance | Retention, archival, access, deletion, legal hold, and evidence-reference durability shall be defined by accountable owners. |
| NFR-013 | Deployment integrity | Artifacts and dependencies shall be verified; production configuration and credentials shall be segregated and controlled. |
| NFR-014 | Operational resilience | Backup, continuity, participant recovery, incident response, and disaster recovery procedures shall be tested. |
| NFR-015 | Scalability | Capacity shall be measured for expected participants, transaction volume, privacy domains, and reconciliation workloads before rollout. |

## 2. Required measurable decisions

Before production approval, owners must define and evidence targets for availability, recovery time and recovery point, throughput, end-to-end and queue latency, reconciliation frequency and age, approval queue limits, retention periods, incident response, and capacity headroom. Targets must be aligned with bank policy and the selected Canton and integration topology.

## 3. Verification evidence

Evidence may include load and resilience test reports, access reviews, threat-model results, privacy analysis, backup/restore exercises, release provenance, operational dashboards, and signed service-owner approvals. A design statement alone does not verify an NFR.