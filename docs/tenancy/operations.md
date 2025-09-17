---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Tenant Operations

## Zweck & Scope
Dieses Dokument beschreibt runbook-orientierte Prozesse fuer Betriebsteams, um Mandanten sicher zu verwalten. Es ergaenzt die unter `md.html?path=tenancy/tenancy.md` definierte Architektur um konkrete Ablaeufe.

## Enterprise-Anforderungen
- 24/7 Bereitschaft fuer tenantrelevante Incidents mit klaren Eskalationsketten.
- Nachweisbare Access Reviews, Kostenberichte und Compliance-Audits pro Tenant.
- Automatisierte Drift-Erkennung und Policy-Enforcement bei Abweichungen.
- Self-Service-Funktionen mit Approval- und Logging-Mechanismus.
- Integration mit Incident-, Change- und Problem-Management (ITIL-kompatibel).

## Technologie-Stack
- n8n Workflows fuer Onboarding/Offboarding und Wartungsfenster.
- Terraform/Argo CD Pipelines fuer Infrastruktur-Aenderungen.
- Neon/Redis Monitoring Dashboards, FinOps Ledger, Alertmanager Hooks.
- PowerShell/CLI-Tools fuer Sonderaufgaben (Backup Restore, Datenexport).
- GitOps Repositories fuer Tenant-Configs und Secrets.

## Architektur & Betrieb
Operations nutzt einen zentralen Control Plane Namespace. Workflows werden ueber Service Accounts mit zeitlichen Berechtigungen ausgefuehrt. Health Monitor prueft Tenant-Endpunkte. Incident-Isolation Scripts kapseln Namespaces und deaktivieren Secrets. Reporting-Jobs schreiben Kennzahlen nach Neon `finops.tenant_ledger` und Compliance Tabellen.

## Schnittstellen & Vertraege
- Onboarding API: `/api/v1/tenants` (create/approve/suspend) mit RBAC, Audit.
- Event Hooks: Kafka Topics fuer Billing, Compliance, Security.
- Reporting: Woechentliche SLA/SLO Berichte an Governance, monatliche Cost Reports an Finance.
- Secrets Rotation: Contract mit Security (Rotation Cycle, Notification Hooks).

## Standard Runbooks
### Tenant Onboarding
1. **Request Intake (Account Manager)**
   - Ticket in ServiceNow mit Business Owner, Compliance Klassifizierung, Region.
2. **Approval (Governance + Security)**
   - Review Policies, Data Residency, Budget; Link zu Risk Register Eintrag.
3. **Provisioning (Ops Engineer)**
   - Terraform Pipeline `tenant-bootstrap` ausfuehren (Namespace, Quotas, SecretStore, RBAC).
   - Argo CD App-of-Apps syncen, Validierung via GitOps Drift Check.
4. **Verification (QA)**
   - Health Monitor Tenant Check, Synthetic Tests, Access Review (Read/Write).
5. **Handover (Account Manager)**
   - Send Onboarding Pack (Dashboards, Contacts, Cost Controls), log in Neon `tenant_events`.

_SLA_: Provisioning < 30 min nach Approval. _Evidence_: Terraform Plan, Argo Sync Logs, Health Monitor Report.

### Tenant Suspension / Isolation
1. Security Incident oder Payment Breach triggern n8n Flow.
2. Ops isoliert Namespace (NetworkPolicies -> deny), pausiert Queues (Redis ACL update).
3. Secrets revoked via Vault, Access Disabled in Entra/Keycloak.
4. Notify Stakeholder (Legal, Finance, Tenant Owner) via Teams + Email Template.
5. Collect Evidence (Logs, Alerts) und aktualisiere Risk Register Status.
6. Freigabe fuer Reaktivierung nur via Governance Approval.

### Cost Overrun Response
1. Alertmanager FinOps Alert -> PagerDuty FinOps Rotation.
2. Review Grafana Tenant Budget Dashboard, identifiziere Service Hotspots.
3. Wende Rate Limit Policy an (Redis Bucket Update) und benachrichtige Tenant.
4. Erstelle FinOps Report (Neon Query) + send an Finance & Governance.
5. Dokumentiere Massnahmen im Tenant Ledger, plane Follow-up Meeting.

## Deliverables
- Runbook Bibliothek (Onboarding, Offboarding, Suspension, Emergency Break Glass) mit Versionskontrolle.
- Playbooks fuer Cost Overruns, Resource Exhaustion, Policy Violations.
- Checklisten fuer Access Reviews, Compliance Evidence Collection, Budget Reviews.
- Automatisierte Tests fuer n8n Workflows und Terraform Pipelines.
- Tenant KPI Dashboard (Activation Time, SLA, Cost, Incident Count).

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Governance ueber Genehmigungs-Workflows und Segregation of Duties.
- Integration in SLO Dashboard fuer Tenant-spezifische KPIs und Error Budgets.
- Abhaengigkeit zu Security fuer Secrets Rotation und Policy Updates.
- Bedarf nach Self-Service Portal fuer Tenant Owner mit RBAC und Audit.

## Links
- [Multi-Tenant Architektur](md.html?path=tenancy/tenancy.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
- [Risk Register](md.html?path=risk/risk.md)
