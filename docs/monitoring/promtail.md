# Promtail Collectors

## Zweck & Scope
Promtail sammelt Logs von Kubernetes-Workloads, Agenten-Services und Host-Systemen und sendet sie an Loki. Es kodiert die Label-Strategie und sorgt fuer sichere Transporte.

## Enterprise-Anforderungen
- Mandantenfreundliche Pipelines mit Whitelisting und PII-Redaktion.
- TLS-geschuetzte Kommunikation (mTLS) mit Client-Zertifikaten.
- GitOps-verwaltete Pipeline-Configs inklusive Review-Prozess.
- Kompatibilitaet mit Sidecar- und DaemonSet-Betrieb.
- Compliance-konforme Retention und Maskierung (Regex, Replacement).

## Technologie-Stack
- Promtail DaemonSet auf Kubernetes, Sidecar-Pattern fuer kritische Dienste.
- File- und Journald-Scraper, HTTP-Receiver fuer Custom Agents.
- Configuration-as-Code in Git mit Kustomize Overlays pro Environment.
- TLS via cert-manager, Secrets via External Secrets Operator.
- Optionaler Export an Kafka/SIEM fuer Security Use Cases.

## Architektur & Betrieb
Promtail Instanzen laufen mit PodSecurity `restricted`, hostPath-Zugriff minimiert. Retry-Queues verhindern Logverlust bei Loki-Ausfall. Health-Monitor prueft Durchsatz und Error-Raten. Deployment ueber Argo CD mit Canary-Rollouts.

## Schnittstellen & Vertraege
- Push-Endpunkt zu Loki `/loki/api/v1/push` (mTLS, Bearer) und optional `/otlp/v1/logs`.
- Pipelines definieren Label-Set: `tenant`, `service`, `stage`, `component`.
- Export von Audit-Logs via Webhook an Governance-Tools.
- Alerts via Alertmanager bei Drop/Retry-Rate >1%.

## Deliverables
- Pipeline-Library (Regex, Label-Sets, Redaction-Regeln) mit Tests.
- Runbooks fuer Pipeline-Debugging, Log-Drops und Maskierungs-Reviews.
- Helm/Kustomize Module fuer Standard- und Tenant-spezifische Deployments.
- Integrationstest-Suite fuer neue Services (Log-E2E).

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Security ueber neue PII-Redaktionsregeln.
- Entscheidung ueber zentralen OTLP-Gateway fuer Nicht-K8s Dienste.
- Abhaengigkeit zu Loki/Alertmanager fuer End-to-End Observability.
- Integration mit Operations Runbooks und Incident Response.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Loki Log-Stores](md.html?path=monitoring/loki.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
