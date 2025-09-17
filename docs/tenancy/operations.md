# Tenant Operations

## Zweck & Scope
Dieses Dokument beschreibt runbook-orientierte Prozesse fuer Betriebsteams, um Mandanten sicher zu verwalten. Es ergaenzt die unter `md.html?path=tenancy/tenancy.md` definierte Architektur um konkrete Ablaeufe.

## Enterprise-Anforderungen
- 24/7 Bereitschaft fuer tenantrelevante Incidents mit klaren Eskalationsketten.
- Nachweisbare Access Reviews, Kostenberichte und Compliance-Audits pro Tenant.
- Automatisierte Drift-Erkennung und Policy-Enforcement bei Abweichungen.
- Self-Service-Funktionen mit eingebautem Approval- und Logging-Mechanismus.
- Integration mit Incident-, Change- und Problem-Management (ITIL-kompatibel).

## Technologie-Stack
- n8n Workflows fuer Onboarding/Offboarding und Wartungsfenster.
- Terraform/Argo CD Pipelines fuer Infrastruktur-Aenderungen.
- Neon/Redis Monitoring Dashboards, FinOps Ledger, Alertmanager Hooks.
- PowerShell/CLI-Tools fuer Sonderaufgaben (z. B. Backup Restore, Datenexport).
- GitOps Repositories fuer Tenant-Configs und Secrets.

## Architektur & Betrieb
Operations nutzt einen zentralen Control Plane Namespace. Workflows werden ueber Service Accounts ausgefuehrt, die nur temporale Berechtigungen erhalten. Health Monitor prueft Tenant-Endpunkte. Incident Isolations-Skripte kapseln Namespaces und deaktivieren Secrets. Reporting-Jobs schreiben Kennzahlen nach Neon.

## Schnittstellen & Vertraege
- Onboarding API: `/api/v1/tenant-operations` (create/approve/suspend) mit RBAC.
- Event Hooks: Kafka Topics fuer Billing, Compliance, Security.
- Reporting: woechentliche SLA/SLO Berichte an Governance, monatliche Cost Reports an Finance.
- Secrets Rotation: Contract mit Security (Rotation Cycle, Notification Hooks).

## Deliverables
- Runbook Bibliothek (Onboarding, Offboarding, Isolation, Emergency Break Glass).
- Playbooks fuer Cost Overruns, Resource Exhaustion, Policy Violations.
- Checklisten fuer Access Reviews und Compliance Evidence Collection.
- Automatisierte Tests fuer n8n Workflows und Terraform Pipelines.

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Governance ueber Genehmigungs-Workflows und SoD.
- Integration in SLO Dashboard fuer Tenant-spezifische KPIs.
- Abhaengigkeit zu Security fuer Secrets Rotation und Policy Updates.
- Bedarf nach Self-Service Portal fuer Tenant Owner.

## Links
- [Multi-Tenant Architektur](md.html?path=tenancy/tenancy.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
