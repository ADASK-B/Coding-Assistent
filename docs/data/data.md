# Data Layer

## Zweck & Scope
Dieser Abschnitt beschreibt die Datenhaltung der Plattform mit Fokus auf transaktionale, operative und Cache-Daten. Kernelemente sind Neon Postgres und Redis, flankiert von Objekt- und Vektorstores.

## Enterprise-Anforderungen
- Mandantengetrennte Schemata, Keys und Ledger fuer Compliance und Kosten.
- Verschluesselung, Backup (PITR) und Audit-Logging fuer alle Datenstroeme.
- Hochverfuegbare Deployments mit Failover-Strategien und Replikationen.
- Datenklassifizierung, Retention Policies und DSGVO-gerechte Exportprozesse.
- Automatisierte Schema- und Migrationspruefungen in CI/CD.

## Technologie-Stack
- Neon Postgres (Serverless/Branching) fuer Transaktionen, Ledger, Configs.
- Redis Cluster (TLS, ACL) fuer Cache, Queues, Rate-Limits, Heartbeats.
- Objektstorage (MinIO/S3/Azure Blob) fuer Artefakte und gro?e Logs.
- Vektorstore (Qdrant) fuer Kontextbeschaffung im RAG-Szenario.
- ETL/Streaming via Debezium, Kafka, n8n fuer Data Sync.

## Architektur & Betrieb
Daten werden ueber Data Access Layer (DAL) und Services angesprochen. Neon nutzt Branching fuer Testdaten und Blue/Green Releases. Redis Cluster laeuft in Hochverfuegbarkeits-Topologie mit Sentinel/Operator. Backup-Jobs sichern Snapshots in verschluesselten Buckets. Schema-Aenderungen durchlaufen GitOps mit Tests.

## Schnittstellen & Vertraege
- API-Ebene: gRPC/REST, die DAL-Contracts einhalten (DTOs, JSON Schema).
- Eventing: Kafka Topics fuer Change Data Capture und Ledger Events.
- RBAC: Policy-Engine (OPA/Cerbos) erzwingt Query/Mutation Scopes.
- Monitoring: Prometheus Exporter fuer Redis/Neon, Grafana Dashboards.

## Deliverables
- Data Architecture Diagramme (Logical/Physical/Access).
- Playbooks fuer Migrations, Failover, PITR und Data Privacy Requests.
- Test-Suiten fuer Schema, Performance und Data Quality.
- Dokumentierte Retention- und Lifecycle-Policies.

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber Multi-Region Setup fuer Neon (Read Replicas).
- Konsolidierung der Vektorstore-Strategie (Qdrant vs. Alternative).
- Abstimmung mit Governance zu Data Access Approvals und Audits.
- Abhaengigkeit zu Operations fuer Backup und DR-Tests.

## Links
- [Redis Details](md.html?path=data/redis.md)
- [Neon Postgres](md.html?path=data/neon.md)
- [Disaster Recovery](md.html?path=dr/dr.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
