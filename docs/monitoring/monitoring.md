# Monitoring & Observability

## Zweck & Scope
Monitoring Stack ueberwacht Agentenplattform Ende-zu-Ende: Metriken, Logs, Traces, Kosten, Health. Ziel ist fruehzeitige Incident-Erkennung und Steuerung von Error Budgets.

## Enterprise-Anforderungen
- Mandantenfaehige Dashboards, Alerting und Reports.
- Vollstaendige Nachverfolgbarkeit (Audit, Budget, Token, Kosten).
- Integration in Incident-, Change- und Compliance-Prozesse.
- Automatisierte Synthetic Checks und Auto-Remediation Hooks.
- Datenschutzkonforme Datenhaltung (Retention, Masking).

## Technologie-Stack
- Prometheus fuer Metrics, Alertmanager fuer Alerts.
- Grafana fuer Visualisierung und OnCall.
- Loki/Promtail fuer Logs, Tempo fuer Traces.
- cAdvisor, Node Exporter fuer Infrastrukturmetriken.
- Health Monitor fuer Synthetic Checks, Status API.
- Redis/Neon fuer Budget Ledger und Alert Persistenz.

## Architektur & Betrieb
Stack laeuft in Observability Namespace, GitOps verwaltet Deployment (Helm/Argo). Traefik Ingress sichert mTLS. Prometheus sammelt Daten via ServiceMonitor, Alertmanager routet nach Teams/PagerDuty/ADO. Grafana hostet Dashboards mit RBAC. Loki/Promtail erfassen Logs; Tempo verlinkt Traces. Health Monitor orchestriert Checks und Fuege Heartbeats in Redis ein.

## Schnittstellen & Vertraege
- Prometheus `/api/v1/query` fuer Metriken, Alertmanager `/api/v2/*` fuer Alerts.
- Grafana API fuer Dashboards, Folder RBAC.
- Loki `/loki/api/v1/query`, Promtail Push `/loki/api/v1/push`.
- Health Monitor `/api/v1/status` fuer Tenants.
- Exporte nach Governance/Compliance (Evidence Packs).

## Deliverables
- Referenzarchitektur (Datenfluesse, Mandantentrennung).
- Dashboard Katalog (Platform, Tenant, FinOps, Security).
- Alert Policies mit Runbooks und Eskalationsmatrix.
- Test Suites fuer Synthetic Checks, Chaos Szenarien, Alert Validierung.

## Offene Punkte & Abhaengigkeiten
- Langzeitarchivierung fuer Logs/Metrics (Thanos, Glacier).
- Automatisierte Cost Brake via Redis/Neon Schwellen.
- Integration mit Evaluation fuer Modellqualitaet KPIs.
- Abhaengig von Operations fuer 24/7 OnCall.

## Links
- [Prometheus Monitoring](md.html?path=monitoring/prometheus.md)
- [Grafana Dashboards](md.html?path=monitoring/grafana.md)
- [Loki Log-Stores](md.html?path=monitoring/loki.md)
- [Health Monitor](md.html?path=monitoring/health-monitor.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
