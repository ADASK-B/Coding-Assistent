# Kubernetes Migration

## Zweck & Scope
Diese Seite beschreibt die Strategie zur Migration von Docker Compose zu einer produktionsreifen Kubernetes Plattform inklusive GitOps, Autoscaling und Multi-Tenant Controls.

## Enterprise-Anforderungen
- Hochverfuegbare Workloads mit HPA/KEDA, PodDisruptionBudgets und Resilience Tests.
- Strikte Security (PSP Nachfolger, NetworkPolicies, Secrets Management).
- Automatisiertes Cluster Lifecycle Management (AKS, Argo CD, Terraform).
- Observability und FinOps Integration fuer Kosten, Kapazitaet, SLOs.
- Reproduzierbare Environments (Dev, Stage, Prod) mit Policy Gates.

## Technologie-Stack
- AKS/On-Prem K8s, Cluster Bootstrap via Terraform + Argo CD App-of-Apps.
- Helm Charts/Kustomize, Argo Rollouts, Flux optional.
- Ingress: Traefik + cert-manager, Service Mesh (Linkerd/Istio) fuer mTLS.
- Operators: Redis, Neon Gateway, External Secrets, Prometheus Stack.
- HPA/KEDA fuer queue-basierte Skalierung, Vertical Pod Autoscaler optional.

## Architektur & Betrieb
Cluster wird per IaC erzeugt, Netzwerk-Segmentation ueber Azure CNI + NetworkPolicies. Namespaces pro Tenant und System Layer. Argo CD verwaltet Deployments, Policy Engine (Gatekeeper) validiert. Observability via Prometheus/Loki, FinOps Tags. DR Szenarien ueber Multi-AZ, Backup Tools (Velero). Upgrade Pfad definiert (Blue/Green Nodes).

## Schnittstellen & Vertraege
- GitOps Repo Struktur (`environments/<stage>`) mit App-of-Apps Manifest.
- Admission Webhooks fuer Policy Checking (OPA/Kyverno).
- Pipeline Contract: `kubectl diff/apply` nur via Argo CD, kein direkter Zugriff.
- Service Mesh mTLS Certificates ueber cert-manager + Vault.

## Deliverables
- Migration Roadmap (milestones, prerequisites, rollback plan).
- Cluster Runbooks (Scaling, Upgrades, Incident Response).
- Helm Chart Library fuer Plattform-Services.
- Security Hardening Guide (NSA/CIS Benchmarks mapping).

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber istio vs. linkerd fuer Service Mesh.
- Abstimmung mit Operations bzgl. 24/7 Support und OnCall.
- Integration mit Quality/Evaluation fuer Performance Tests.
- Abhaengigkeit zu Deployment Strategy und Tenant Architektur.

## Links
- [Deployment Strategy](md.html?path=deployment/deployment.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Security Framework](md.html?path=security/security.md)
- [SLO Framework](md.html?path=slo/slo.md)
