# Grafana Dashboards

## Zweck & Scope
Grafana dient als mandantenfaehige Visualisierungsschicht fuer den kompletten Observability Stack. Dashboards, Playbooks und OnCall-Flows ermoeglichen Betrieb, FinOps und Security schnelle Diagnose entlang der in `md.html?path=monitoring/monitoring.md` dokumentierten KPIs.

## Enterprise-Anforderungen
- Tenant-Folder mit RBAC pro Rolle (Ops, FinOps, Security) inklusive Audit-Logging.
- SSO-Integration (Entra ID/Keycloak) und verpflichtende MFA fuer OnCall-Gruppen.
- Review- und Freigabeprozess fuer Dashboard- und Alert-Aenderungen via GitOps.
- Hochverfuegbare Deployment-Topologie (2+ Replikas, Persistenz fuer State).
- Integrationsfahigkeit mit Incident- und Ticket-Systemen (ADO, PagerDuty, ServiceNow).

## Technologie-Stack
- Grafana OSS/Enterprise im Kubernetes-Cluster mit PostgreSQL-Backend fuer Konfigurationen.
- Grafana OnCall und Alerting Rules, die Prometheus, Loki, Tempo und externe Webhooks anzapfen.
- Provisionierung via Helm + ConfigMaps/Secrets, automatisiert durch Argo CD.
- Dashboards als JSON/YAML in Git-Repos (Infrastructure-as-Dashboards) mit Linting.
- Secret-Management ueber External Secrets Operator, TLS Zertifikate via cert-manager.

## Architektur & Betrieb
Grafana laeuft in der Observability-Namespace mit Horizontal Pod Autoscaler (HPA) und optionalem Redis Cache. Persistente Volumes halten Data Sources und Alert State. Ingress via Traefik sichert mTLS, WAF-Regeln und Rate-Limits. Synthetic Checks pruefen Rendertauglichkeit und Response-Zeiten, waehrend Backups ueber Velero/Restic erfolgen.

## Schnittstellen & Vertraege
- HTTPs Dashboards/API mit OAuth2/OIDC und Mandanten-spezifischen Foldern.
- Alert Webhooks zu Teams, Slack, PagerDuty und ADO Service Hooks.
- Datasource-Vertraege: Prometheus (PromQL), Loki (LogQL), Tempo (TraceQL), RedisTimeSeries.
- Audit-Events via Loki/Neon fuer Nachweispflichten.

## Deliverables
- Dashboard-Katalog inkl. Standard-Folder (Platform, Tenant, Cost, Security).
- Betriebshandbuch mit Backup/Restore, HPA-Tuning und Incident-Workflows.
- Terraform/Helm-Module fuer Self-Service-Bereitstellung neuer Tenants.
- Runbooks fuer OnCall-Handovers, Alert-Triage und Playbook-Aktualisierungen.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber Enterprise-Lizenz fuer erweiterte RBAC und Reporting.
- Abstimmung mit Governance bzgl. Dashboard-Reviews und Freigabeworkflows.
- Evaluierung von DSGVO-konformen Retention-Policies fuer Dashboard-Snapshots.
- Integration der Health-Monitor-Metriken als nativer Datasource.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Prometheus Monitoring](md.html?path=monitoring/prometheus.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Quality Framework](md.html?path=quality/quality.md)
