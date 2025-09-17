# Azure DevOps Integration

## Zweck & Scope
Beschreibt die Anbindung der Plattform an Azure DevOps fuer Boards, Repos, Pipelines und Service Hooks. Ziel ist ein Ende-zu-Ende Automationsfluss fuer Agenten.

## Enterprise-Anforderungen
- Mandantenfaehige Projekte mit RBAC, Branch Policies, Service Connections.
- Auditierbare Workflows, verlinkte Work Items, Compliance Checklists.
- Sicherer Webhook-Betrieb (mTLS, IP Filter, Secrets, Retries).
- Automatisierte Pipeline-Templates und Quality Gates.
- Synchronisation mit Governance (Approvals, Evidence Packs).

## Technologie-Stack
- Azure DevOps REST API, Service Hooks, Git Repos, YAML Pipelines.
- n8n/Azure Functions fuer Webhook Handling und Orchestrator Dispatch.
- OAuth Apps/Service Principals (Entra ID) fuer Auth.
- Terraform Module fuer Projekt-, Repo-, Pipeline-Provisioning.
- Monitoring ueber Application Insights/Prometheus Exporter.

## Architektur & Betrieb
Ingress Webhooks landen im Gateway, werden validiert (HMAC) und nach Redis Queue geschrieben. Orchestrator mappt Work Items, Branch Policies enforced via PR Templates, Checks. Pipeline Templates (Starter YAML) werden ueber GitOps ausgerollt. Health Monitor ueberwacht Hook Durchsatz.

## Schnittstellen & Vertraege
- Service Hook Endpoints `/webhooks/ado/{event}` (JSON, HMAC-SHA256).
- Pipeline Contracts: `azure-pipelines.yml` Template mit Stage/Jobs.
- Branch Policy: Build Validation, PR Reviewer, Security Scans.
- Reporting: Work Item Links, Tags, Cost Ledger Updates.

## Deliverables
- Provisioning Scripts (Terraform/Bicep) fuer Projekte, Repos, Pipelines.
- Hook Handler Code (TypeScript) mit Tests und Replay Mechanismen.
- Dokumentation fuer Branch Strategies, PR Templates, Policy Setup.
- Monitoring Dashboards fuer Hook Latenz, Error Rate, Pipeline Erfolg.

## Offene Punkte & Abhaengigkeiten
- Integration mit anderen SCMs (GitHub/GitLab) via Adapter.
- Abstimmung mit Governance fuer Branch Policy Standards.
- Abhaengigkeit zu Security fuer Secrets/Service Connections.
- Integration mit Quality/Evaluation fuer Test Metrics.

## Links
- [Plattform APIs](md.html?path=api/api.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Roadmap](md.html?path=roadmap.md)
