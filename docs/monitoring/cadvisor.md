# cAdvisor Metrics

## Zweck & Scope
cAdvisor liefert Container- und Node-Metriken fuer CPU, Memory, Disk und IO. Die Daten flie?en in den Observability-Stack, um Kapazitaetsplanung sowie Auto-Scaling im Sinne von `md.html?path=k8s/k8s.md` zu ermoeglichen.

## Enterprise-Anforderungen
- Sammler pro Node mit garantierten Ressourcen-Limits zur Vermeidung von Selbst-Interferenz.
- Mandantenfreundliche Labeling-Strategien zur Trennung von Produktions- und Sandbox-Workloads.
- Verschluesselte Kommunikation zu Prometheus und Alertmanager via mTLS.
- Kompatibilitaet mit FinOps-Reports fuer Ressourcenverbrauch pro Tenant.
- HPA/KEDA-Feeding mit stabilisiertem Sampling (<15s) und Ausreisser-Filter.

## Technologie-Stack
- cAdvisor als DaemonSet auf allen Kubernetes-Nodes inklusive GPU-Pools.
- Metrics Endpoint `/metrics/cadvisor` im Prometheus-Format.
- Integration mit Node Exporter und Kube-State-Metrics fuer Gesamtbild.
- Helm/Kustomize Module fuer Deployment, verwaltet durch Argo CD.
- Optionaler Export nach Neon fuer historische Kapazitaets-Analysen.

## Architektur & Betrieb
Das DaemonSet nutzt tolerations/nodeSelectors, um auf Worker- und GPU-Nodes zu laufen. Ressourcengarantien verhindern Evictions. Traefik Service Mesh stellt mTLS sicher. Synthetic Checks pruefen Liveness/Readiness, waehrend Redis Kurzzeitpuffer fuer Peak-Analyse bereitstellt.

## Schnittstellen & Vertraege
- Prometheus Scrape-Vertrag (Port 4194) mit Auth durch ServiceAccount + TLS.
- Labels: `tenant`, `app`, `nodepool`, `environment` fuer Downstream-Analysen.
- Export-Pipelines zu Grafana Dashboards und Alertmanager Rules.
- Datenbereitstellung fuer FinOps-Berichte und Capacity Runbooks.

## Deliverables
- Deployment-Manifest inkl. Security Context und PodSecurity Standard (restricted).
- Dokumentierte Metrics-Mappings fuer Capacity/SLO Dashboards.
- Test-Cases fuer Metrics-Verfuegbarkeit und Label-Konsistenz.
- Runbooks fuer Node-Rotation, Kernel-Upgrades und GPU-Spezifika.

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Platform-Team zu Node Hardenings und Pod Security Policies.
- Entscheidung ueber langfristige Speicherung der Kapazitaetsdaten.
- Integration mit Health-Monitor fuer Node-Drift-Alerts.
- Abhaengigkeit zu Node Exporter fuer Gesamtabdeckung.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Node Exporter](md.html?path=monitoring/node-exporter.md)
- [Kubernetes Deployment](md.html?path=k8s/k8s.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
