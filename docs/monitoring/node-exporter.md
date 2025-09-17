# Node Exporter

## Zweck & Scope
Node Exporter liefert systemnahe Metriken fuer Linux-Hosts, die Agenten und Infrastruktur-Services betreiben. Er bildet die Grundlage fuer Kapazitaetsplanung, Hardware-Monitoring und Security-Baselines.

## Enterprise-Anforderungen
- Read-only Zugriff auf Host-Metriken mit minimiertem Angriffsvektor.
- TLS-gesicherte Prometheus-Scrapes und IP-Allowlists.
- Mandanten-Sichtbarkeit ueber vereinheitlichte Labels (tenant, environment, role).
- Kompatibilitaet mit Compliance-Anforderungen (Audit via Neon, Langzeit-Archiv).
- Ueberwachung kritischer Kernel-Parameter und Filesystem-Backups.

## Technologie-Stack
- Node Exporter DaemonSet auf Kubernetes Nodes und Bare-Metal Runnern.
- Systemd-Unit fuer Nicht-K8s Hosts (z. B. n8n, Health Monitor, Bastion).
- ServiceMonitor Definition via Prometheus Operator, gemanagt durch Argo CD.
- TLS-Zertifikate via cert-manager, Secrets via External Secrets Operator.
- Integration mit Redis fuer kurzfristige Drift-Erkennung (Baseline vs. Live).

## Architektur & Betrieb
Deployment erfolgt mit dedizierten ServiceAccounts, PodSecurity Standard `restricted` und HostPID-Freigabe nur wenn erforderlich. Scrapes laufen ueber Prometheus mit 15s-Intervall; Alerts pruefen Disk/CPU/Memory Limits. Backups sichern Konfigurationen via GitOps.

## Schnittstellen & Vertraege
- HTTPS Endpoint `/metrics` mit BasicAuth/OAuth mTLS Proxy.
- Labels fuer SLO/SLA: `tenant`, `cluster`, `nodepool`, `os`, `criticality`.
- Event-Weiterleitung an Alertmanager und Health Monitor.
- Export nach Neon fuer Trend-Analysen.

## Deliverables
- Deployment- und Konfig-Dateien fuer K8s und Bare-Metal.
- Security Review (Least Privilege, PodSecurity, Secrets Handling).
- Baseline Dashboards fuer Hardware-Health.
- Runbooks fuer Node-Replacement, Kernel-Patches, Agent-Drift.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber kombinierten Exporter (Node + Process) fuer Runner.
- Abstimmung mit Security bzgl. Hardening der Host-Zugriffe.
- Integration in Capacity Planning (Quality & FinOps) Dashboards.
- Abhaengig von Prometheus Operator und Zertifikats-Infrastruktur.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [cAdvisor Metrics](md.html?path=monitoring/cadvisor.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
