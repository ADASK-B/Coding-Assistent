---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Deployment Strategy

## Zweck & Scope
Dieses Dokument skizziert die Bereitstellung der Plattform in lokalen, Test- und Produktionsumgebungen. Es verbindet One-Command Docker Compose Setups mit dem geplanten K8s Rollout.

## Enterprise-Anforderungen
- Wiederholbare Deployments mit GitOps und verifizierbaren Artefakten.
- Sicherheitskontrollen (signierte Images, Vulnerability Scans, Secrets Handling).
- Zero-Downtime Releases mittels Blue/Green oder Canary.
- Dokumentierte Rollback- und Disaster-Recovery-Strategien.
- Uebernahme in CI/CD (ADO Pipelines) mit Approval Gates und Audit Trail.

## Technologie-Stack
- Docker Compose (local-dev, demo) mit Health Monitor, Grafana, Prometheus.
- Kubernetes (AKS/on-prem) mit Argo CD, Helm Charts, Kustomize Overlays.
- Traefik als Ingress Controller, cert-manager fuer TLS, External Secrets Operator.
- OCI Registry (ACR/GHCR), Cosign Signaturen, SBOM via Syft.
- CI/CD Pipelines in Azure DevOps, optional GitHub Actions fuer OSS.

## Architektur & Betrieb
Lokale Environments nutzen Compose (`docker-compose up platform`). Staging/Prod laufen auf Kubernetes mit Namespaces pro Stage. Argo CD App-of-Apps verwaltet Services. Pipelines bauen Images, scannen, signieren und deployen an Git-Tag. Health Monitor prueft Post-Deploy Checks; Rollback-Pfade definieren Container Version + DB Branch.

## Schnittstellen & Vertraege
- CI/CD Pipeline Contracts: Build (Dockerfile), Scan (trivy), Sign (cosign), Deploy (helmfile).
- Deployment API Hooks zu Operations fuer Change Management.
- Secrets & Config Contracts via GitOps (values.yaml, SecretStoreRef).
- Status Reports an Governance (Change Tickets, Evidence Packs).

## Deliverables
- Compose Files und README fuer lokale Reproduktion.
- Helm/Kustomize Charts inkl. Values Templates pro Environment.
- Pipeline YAMLs (ADO) mit Build/Test/Deploy Stages und Quality Gates.
- Runbooks fuer Deployment, Rollback, Approval Workflow.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber progressive Delivery Tool (Argo Rollouts vs. Flagger).
- Abstimmung mit K8s Team zu Cluster-Policies und Node Pools.
- Integration mit SLO/SLA Dashboards fuer Deploy Impact.
- Abhaengigkeit zu Data Layer fuer Schema Migrationen.

## Links
- [Kubernetes Migration](md.html?path=k8s/k8s.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [Disaster Recovery](md.html?path=dr/dr.md)

