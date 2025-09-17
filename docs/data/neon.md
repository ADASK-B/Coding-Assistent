# Neon Postgres

## Zweck & Scope
Neon Postgres bildet das transaktionale Herzstueck fuer Agentenstatus, Ledger, Tenants und Configuration Management. Es ermoeglicht Branching und serverlose Skalierung fuer Dev/Test/Prod.

## Enterprise-Anforderungen
- Mandantengetrennte Schemata, Roles und Encryption Keys.
- Point-in-Time-Recovery (PITR) und Geo-Replication fuer Business Continuity.
- DSGVO-konforme Data Residency (EU Region) und Audit Logging.
- Automatisierte Schema-Validierung, Migration Rollbacks und Drift Detection.
- Integration in FinOps (Kosten je Branch/Tenant) und Compliance Reports.

## Technologie-Stack
- Neon Serverless auf EU-Cluster, optional Self-Hosted Compute.
- Access Layer via Prisma/TypeORM (Typescript) plus Python SDKs.
- Migrationstools: Sqitch/Flyway/Liquibase in GitOps Pipeline.
- Monitoring: pg_stat_statements, Neon Insights, Prometheus Postgres Exporter.
- Backup: Neon PITR, Offsite Snapshots (encrypted S3), Testdaten via Branches.

## Architektur & Betrieb
Compute/Storage separation von Neon, Multi-Branch Setup fuer Dev/Stage/Prod. Connection Pools (PgBouncer) laufen in Kubernetes. Secrets ueber Vault, Rotation automatisiert. Branch Promotion via CI, Data Masking Jobs fuer Non-Prod. Failover-Plans definieren RTO/RPO.

## Schnittstellen & Vertraege
- Connection Strings mit Service Accounts (Least Privilege Roles).
- API: Neon Control Plane fuer Branch/Project Automation (n8n Connector).
- CDC: Logical Replication via Debezium -> Kafka -> Downstream (Ledger, Data Lake).
- Reporting: SQL Views fuer Compliance, FinOps, Risk.

## Deliverables
- Datenmodell-Dokumentation (ERD, Schema, Stored Procedures).
- Migration Playbooks inkl. Rollback/Blue-Green Strategien.
- Security Controls (Row-Level Security, Column Masking, Audit Policies).
- Performance Benchmarks und Tuning-Guides.

## Offene Punkte & Abhaengigkeiten
- Evaluierung von Read Replicas fuer Reporting/Analytics.
- Definition von Maskierungsregeln fuer Nicht-Prod Branches.
- Abstimmung mit Operations fuer DR-Tests und Monitoring Alerts.
- Koordination mit Data/Evaluation fuer Benchmark-Szenarien.

## Links
- [Data Layer](md.html?path=data/data.md)
- [Redis Cluster](md.html?path=data/redis.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
- [Disaster Recovery](md.html?path=dr/dr.md)
