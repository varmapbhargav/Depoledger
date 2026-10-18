# Operational Controls and Runbook Guidance

## 1. Daily operations

- Review pending approvals and aged workflows.
- Review unknown payment/settlement outcomes before retry.
- Monitor failed integrations, dead letters, and workflow recovery queues.
- Review reconciliation mismatches, owners, deadlines, and escalations.
- Monitor participant, key service, core banking, GL, and payment connectivity.
- Confirm scheduled servicing and maturity jobs completed with expected counts.
- Retain operational evidence and protect sensitive data in logs and reports.

## 2. Exception management

Every case must have a unique ID, operation references, severity, owner, opened time, evidence, next action, remediation deadline, escalation, and auditable resolution. High-risk financial exceptions require independent review under approved policy. Never resolve a mismatch by direct, undocumented balance edits.

## 3. Safe retry checklist

Before replaying any command: verify original operation ID and digest; inspect durable workflow history; query the external authority for current outcome; confirm deduplication/idempotency is active; verify no competing operation or reservation exists; ensure approval and policy remain valid; select the documented retry/resume point; record operator and reason. If outcome remains unknown, do not retry irreversible effects—escalate.

## 4. Incident categories

- Unauthorized or suspicious financial operation.
- Key, credential, or service identity compromise.
- Canton participant/network outage or inconsistent submission status.
- Core banking, payment, GL, or compliance provider outage.
- Duplicate/incorrect posting or payout.
- Privacy or data integrity incident.
- Reconciliation backlog or systemic mismatch.
- Maturity/servicing job failure.

## 5. Incident response outline

1. Declare and classify incident under institutional policy.
2. Contain safely (including scoped pause/restriction where authorized) without deleting evidence.
3. Establish transaction and participant impact from authoritative evidence.
4. Notify accountable business, security, operations, legal/compliance, and provider teams as required.
5. Recover through approved runbooks; do not bypass maker-checker controls without a documented emergency process.
6. Reconcile affected records and independently verify correction.
7. Preserve evidence, timeline, decisions, and external communications.
8. Complete root-cause analysis, required notifications, and corrective actions.

## 6. Business continuity and recovery

Define recovery time/recovery point objectives with owners. Test application/workflow recovery, operational database restoration where applicable, message replay, Canton participant recovery, key service continuity, provider failover, and reconciliation after restoration. A backup is not proven until a restore exercise is successful. Follow the selected Canton and bank infrastructure procedures.

## 7. Runbook template

For each operational procedure document: trigger and severity; prerequisites/authority; systems and dashboards; stepwise diagnosis; safe actions and prohibited actions; verification evidence; escalation contacts/roles; recovery/rollback limits; customer/business communications; closure criteria; and review date/owner.