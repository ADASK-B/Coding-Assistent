---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Multi-Tenant Architektur

## Zweck & Scope
Dieses Dokument definiert Isolationsprinzipien, Provisioning-Prozesse und Governance fuer Mandanten. Ziel ist kontrollierte Bereitstellung, konsistente Ressourcen-Quotas und sicheres Offboarding.

## Enterprise-Anforderungen
- Strikte Isolation auf Netzwerk-, Namespace-, Datenbank- und IAM-Ebene.
- Konforme Datentrennung gem. GDPR/EU-AI-Act mit Audit-Trails und Data-Residency.
- Automatisiertes Tenant-Lifecycle-Management (Create, Update, Suspend, Delete).
- Kosten- und Budgetkontrolle pro Tenant inkl. Alerting und Quoten.
- Self-Service-Fahigkeit mit Genehmigungs-Workflow und Access Reviews.

## Technologie-Stack
- Provisionierung via Argo Project Factory, Terraform/Bicep Module.
- Kubernetes Namespaces + ResourceQuotas + LimitRanges + NetworkPolicies.
- Neon Schema pro Tenant, Redis ACLs und Keyspaces, S3 Buckets mit IAM Policies.
- Identity Federation ueber Entra/Keycloak, ADO/GitHub Project Templates.
- Monitoring Hooks: Prometheus Tenant Labels, Grafana Folders, Ledger Exporte.

## Architektur & Betrieb
Tenant Onboarding erfolgt ueber Self-Service Portal (n8n Flow) mit Approval. Workspaces werden ueber K8s Namespace + GitOps App-of-Apps angelegt. Secrets werden ueber dynamic Vault Secrets verteilt. Kosten-Ledger schreibt Metriken nach Neon. Offboarding automatisiert Revocation, Backups, Retention.

## Schnittstellen & Vertraege
- Tenant API `/api/v1/tenants` (create/update/delete) mit RBAC und Audit.
- Provisioning Events via Kafka fuer Downstream Systeme (Ops, Billing).
- Contract Templates fuer Namespace YAML, Quota, RBAC, SecretStore.
- Reporting Schnittstelle zu Governance/Compliance (Tenants, Owner, Status).

## Deliverables
- Tenant Blueprint (Namespace, Quotas, Secrets, Observability, Budget).
- Runbooks fuer Onboarding, Offboarding, Suspension, Incident Isolation.
- Automation-Skripte (Terraform/Argo Templates) fuer Tenant Deployments.
- Audit Report Format (Access Reviews, Cost Reports, Data Residency).

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber Self-Service UI vs. CLI fuer Tenant Requests.
- Abstimmung mit Finance/Legal zu Abrechnungsmodellen und Kontrakten.
- Abhaengigkeit zu Operations fuer Incident-Isolation und Runbooks.
- Integration mit Risk Register fuer Tenant-spezifische Risiken.

## Links
- [Tenant Operations](md.html?path=tenancy/operations.md)
- [Core Plattform](md.html?path=core/core.md)
- [Data Layer](md.html?path=data/data.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)

