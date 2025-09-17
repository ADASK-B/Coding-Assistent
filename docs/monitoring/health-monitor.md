# Health Monitor

## Zweck & Scope
Der Health Monitor orchestriert Synthetic Checks, Heartbeats und Status-Seiten fuer alle Plattform-Services. Er spiegelt den Live-Zustand fuer Operations, Tenants und Incident-Commander wider.

## Enterprise-Anforderungen
- Aggregierte Health-Checks fuer 16+ Services mit SLA- und Error-Budget-Auswertung.
- Authentifizierte Status-API mit Mandantenfiltern und Audit-Logs.
- Automatisierte Eskalationen an Alertmanager und Operations bei Fehlern.
- Resiliente Architektur (Active/Standby, Persistenz fuer Check-Historie).
- Datenschutzkonforme Oeffentlichkeit (Redaktion sensibler Daten auf Public Status Page).

## Technologie-Stack
- Custom Health-Monitor-Service (TypeScript/Node) mit Redis Queue und Neon Persistenz.
- Integration mit Grafana/Prometheus fuer Vergleich Live vs. Metrics.
- Docker Compose fuer lokale Checks, Kubernetes Deployment fuer Produktion.
- TLS und OAuth2 via Traefik Ingress, Secrets ueber External Secrets Operator.
- Webhook-Adapter fuer PagerDuty, Teams, ServiceNow.

## Architektur & Betrieb
Scheduler dispatcht Checks (HTTP, gRPC, Workflow) an Worker Pools. Ergebnisse werden in Redis gepuffert und nach Neon persistiert. Status-API liefert JSON und HTML. Canary-Checks pruefen sich selbst (self-test). HA durch Multi-Replica Deployment und Leader Election.

## Schnittstellen & Vertraege
- `/api/v1/status` (mandantenfaehig, OAuth2) und `/api/v1/checks` fuer Verwaltung.
- Webhook-Kontrakte zu Alertmanager, Orchestrator und Incident-Robotern.
- Event-Publikation via Kafka fuer Reporting und Compliance.
- SLA: Fehlende Heartbeats werden binnen 30s gemeldet.

## Deliverables
- Check-Katalog mit Ownern, Frequenzen und Eskalationspfaden.
- Runbooks fuer Incident-Handling, Maintenance Windows, Self-Healing.
- Dashboard-Integration (Grafana) inklusive Heatmaps und Timeline.
- Testsuite fuer Check-Plugins und Failover-Szenarien.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber oeffentliche Status-Seite vs. Tenant-internes Portal.
- Erweiterung der Check-Plugins (gRPC, DB, LLM-Latenz) fuer neue Services.
- Integration in Operations- und Compliance-Berichte.
- Abhaengigkeit zu Redis und Neon fuer Datenhaltung.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Data Layer Uebersicht](md.html?path=data/data.md)
- [SLO Framework](md.html?path=slo/slo.md)
