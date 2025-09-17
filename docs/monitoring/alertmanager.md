# Alertmanager Service

## Zweck & Scope
Alertmanager orchestriert Alarmfluesse aus Prometheus, Loki und benachbarten Quellen. Er uebersetzt technische Signale in zielgerichtete Eskalationen, die den in `md.html?path=monitoring/monitoring.md` beschriebenen Betrieb sicherstellen.

## Enterprise-Anforderungen
- Mandantenfaehige Routing-Trees mit RBAC, Quiet Times und Escalation Policies.
- Signierte Konfigurationspakete (GitOps) und revisionssichere Versionierung.
- Hochverfuegbarer Clusterbetrieb (>=3 Instanzen) mit Quorum Sync.
- DSGVO- und SOC2-konforme Speicherung von Alert-History und Annotations.
- Automatisierte Integration in Incident- und Ticket-Systeme.

## Technologie-Stack
- Alertmanager Cluster auf Kubernetes, bereitgestellt via Helm + Argo CD.
- Konfiguration durch GitOps-Repository mit Kustomize-Overlays und Policy-Checks.
- Receivers: Teams, Slack, PagerDuty, ServiceNow, Email, Webhooks.
- Secret-Verwaltung via External Secrets Operator, TLS-Zertifikate per cert-manager.
- Anbindung an Redis fuer Rate-Limits von Flapping Alerts.

## Architektur & Betrieb
Der Cluster nutzt StatefulSets mit Persistent Volumes fuer Silences und Notification Logs. Traefik Ingress erzwingt mTLS und IP Allowlisten. Synthetic Tests pruefen Alert-Pfade Ende-zu-Ende. Health-Monitor erfasst Queue-Laengen und liefert Heartbeat-Probes.

## Schnittstellen & Vertraege
- `/api/v2/alerts`, `/api/v2/silences` fuer API-basierte Steuerung (OAuth2 gesichert).
- Webhook-Vertraege mit Orchestrator/Operations fuer Auto-Remediation.
- Event-Streaming zu Kafka fuer Compliance-Logging.
- SLA: Zustellung aktiver PagerDuty-Alerts <30s (P95) nach Trigger.

## Deliverables
- Routing-Tree-Blueprints (Tenant, Environment, Kritikalitaet) mit Notfallketten.
- Runbooks fuer Silence-Management, Escalation-Overrides und Incident-Reviews.
- Test-Suite (Unit + Synthetic) fuer Alert-Verarbeitung inklusive Anti-Flap.
- Dokumentiertes Incident-Postmortem-Template mit KPI-Export.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber Self-Service-Silences (UI) vs. Approval-Workflow.
- Abstimmung mit Governance zu Audit-Eskalationen und Policy-Verstoessen.
- Integration mit Health-Monitor fuer Auto-Remediation Hooks.
- Abhaengig von Prometheus/Loki Rulesets fuer konsistente Labels.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Prometheus Monitoring](md.html?path=monitoring/prometheus.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
