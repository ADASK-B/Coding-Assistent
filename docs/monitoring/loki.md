# Loki Log-Stores

## Zweck & Scope
Loki sammelt containerisierte Logs und Agenten-Events, um Ursachenanalysen entlang der Dev- und Runtime-Fluesse zu ermoeglichen. Es ergaenzt die in `md.html?path=monitoring/monitoring.md` definierte Observability-Basis mit mandantengetrennten Log-Pipelines.

## Enterprise-Anforderungen
- Mandanten-Labeling mit strikten Abfragen (LogQL) und DSGVO-konformer Retention.
- Verschluesselte Transporte (mTLS) zwischen Promtail, Loki und Gateway.
- Immutable Storage mit Backup- und Exportoptionen fuer Forensik.
- Alerting-Hooks fuer Security Events (Anomalien, Policy-Verletzungen).
- Skalierbarkeit fuer >2 TB/Tag Logvolumen mit Queriedauer <5s (P95).

## Technologie-Stack
- Loki Microservices-Modus (Distributor, Ingester, Querier, Query-Frontend) auf Kubernetes.
- Objektstorage (S3/MinIO) fuer Chunk-Layer, Azure Blob als Alternative.
- Index in DynamoDB/Azure Table Storage oder boltdb-shipper je Deployment.
- Promtail/Fluentbit als Agenten mit Multi-Tenant-Pipelines und GELF/OTLP Support.
- Tempo-Integration fuer Trace-Korrelation, SIEM-Export via Kafka Connector.

## Architektur & Betrieb
Loki wird ueber Helm bereitgestellt, horizontal skaliert und ueber Argo CD verwaltet. Ingress hinter Traefik enforce't WAF-Regeln, Rate-Limits und IP-Allowlists. Compactor-Jobs laufen zeitgesteuert, waehrend Backups per Versioning und Lifecycle-Policies des Objectstores abgesichert werden. Health-Monitor prueft Head/Tail-Lags.

## Schnittstellen & Vertraege
- HTTPs API fuer LogQL-Abfragen, gesichert mit OAuth2-Scopes.
- Push-Pfade: `/loki/api/v1/push` fuer Promtail, `/otlp/v1/logs` fuer OTel Collector.
- Exporte via S3/SQS/Kafka fuer Security Analytics und Audit Reporting.
- Querverknuepfung zu Grafana Dashboards und Alertmanager Routes.

## Deliverables
- Multi-Tenant Logging-Konzept inkl. Schemas, Labels und Default-Queries.
- Runbooks fuer Incident Response, Log-Retention, DSAR/Export-Anfragen.
- Terraform/Helm-Vorlagen fuer Loki + Objectstore + IAM Policies.
- Tests fuer Pipeline-Konfigurationen (Regex, Drop/Keep, Redaction).

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber zentrale SIEM-Anbindung (Sentinel, Splunk) via Exporter.
- Abstimmung mit Compliance bzgl. Log-Retention je Datenklasse.
- Performance-Benchmarks fuer Query-Frontend bei Spitzenlast.
- Abhaengigkeiten zu Promtail/Health Monitor fuer Heartbeat-Labels.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Promtail Collectors](md.html?path=monitoring/promtail.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
