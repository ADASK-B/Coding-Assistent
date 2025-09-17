# Redis Cluster

## Zweck & Scope
Redis dient als hochperformanter In-Memory Layer fuer Caching, Rate-Limiting, Queueing und temporale Agenten-Zustaende. Es entlastet Neon und reduziert Latenzen.

## Enterprise-Anforderungen
- TLS-geschuetzter Clusterbetrieb mit ACLs und Mandanten-Keyspace-Trennung.
- Automatisierte Failover-Strategien (Redis Operator/Sentinel) und Pufferung.
- Audit Logging fuer administrative Befehle und Zugriff (ACL Log).
- Policy-gerechte TTLs, um personenbezogene Daten zu minimieren.
- Observability fuer Memory-Hotspots, Evictions, Slowlog, Keys-Per-Tenant.

## Technologie-Stack
- Redis OSS/Enterprise Cluster mit Redis Operator auf Kubernetes.
- Persistence ueber AOF+RDB Snapshots, gespeichert in verschluesselten Buckets.
- Redis Streams fuer Task Queues, RedisJSON/RediSearch optional fuer RAG.
- Rate-Limiter Module, Bloom Filter fuer deduplizierte Events.
- Exporter (redis-exporter) fuer Prometheus, Integration mit Grafana.

## Architektur & Betrieb
Cluster nutzt drei Shards mit Replica pro Shard. NodeSelectors stellen sicher, dass Redis auf High-Memory Nodes laeuft. Service Mesh (mTLS) und Network Policies schuetzen Zugriffe. Backups werden per CronJob erzeugt und in Objektstorage repliziert. Failover Tests sind Teil der DR-Drills.

## Schnittstellen & Vertraege
- TLS-Endpunkte (cluster mode) via Service Mesh, Auth ueber ACL/Service Accounts.
- Contracts fuer Datenstrukturen: Keys prefixed mit `tenant:service:*`.
- Stream-Vertraege fuer Orchestrator, Health Monitor, Alertmanager Hooks.
- Integration mit Neon fuer Ledger Sync (ETL).

## Deliverables
- Redis Usage Catalogue (Keyspaces, TTLs, Owner, Sensitivitaet).
- Betriebsrunbooks fuer Failover, Scaling, Backup/Restore, ACL Rotation.
- Load-/Failover-Testreports fuer DR/SLO Anforderungen.
- Policy-Dokumentation fuer Rate-Limits und Quotas.

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Governance ueber Sensible Keyspaces und Retention.
- Entscheidung ueber Redis Enterprise vs. OSS + Operator.
- Abhaengig von Operations fuer DR-Tests und Alerting.
- Integration mit Evaluation fuer Performance Benchmarks.

## Links
- [Data Layer](md.html?path=data/data.md)
- [Neon Postgres](md.html?path=data/neon.md)
- [Disaster Recovery](md.html?path=dr/dr.md)
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
