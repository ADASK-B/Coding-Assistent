# Core Plattformdienste

## Zweck & Scope
Core Layer stellt synchronen Rueckgrat fuer Auth, Mandantenkontext, Persistenz und Routing bereit. Sie ermoeglicht, dass Agenten dem Entwickler-Workflow aus `md.html?path=goal/goal.md` folgen.

## Enterprise-Anforderungen
- Mandantentrennung auf API-, Daten-, Identity-Ebene.
- HA und Rollback-Faehigkeit fuer Gateway, Orchestrator, Adapter.
- Auditierbarkeit aller API- und Datenzugriffe; Compliance Evidence.
- Schutz sensibler Daten (PII, Secrets) via Verschluesselung, Masking.
- Skalierbarkeit fuer >100 parallele Agenten-Fluesse, Rate Limiting, Budget.

## Technologie-Stack
- Gateway (Traefik) mit OAuth2, Rate Limits, Request Quotas, mTLS.
- Orchestrator (TypeScript) fuer Workflow Steuerung, Queue Management (Redis).
- Adapter: Azure DevOps, GitHub, GitLab, n8n, Webhooks.
- Data Layer: Neon Postgres, Redis Cluster, Qdrant (RAG), Azurite (Blob Emu).
- LLM Layer: LLM Router, vLLM/TGI, Ollama Dev Mode, ngrok fuer Tunnel.
- Supporting Services: Docs Generator, Pipeline Factory, Capability Registry.

## Architektur & Betrieb
Traefik terminiert TLS, delegiert an Gateway. Gateway validiert Auth, orchestriert Tasks via Orchestrator. Orchestrator nutzt Event Hub (Kafka/RabbitMQ) fuer langlaufende Jobs, Redis Streams fuer Queues. Persistent Data liegt in Neon (Schema pro Tenant). Ollama dient lokalen Tests, Router abstrahiert Cloud Provider. Azurite stellt Blob Storage Simulation. GitOps (Argo CD) verwaltet Deployments, Health Monitor prueft Heartbeats.

## Schnittstellen & Vertraege
- Gateway REST/gRPC APIs (`/api/v1/tasks`, `/api/v1/agents`, `/api/v1/context`).
- Webhooks: ADO/GitHub/GitLab -> Gateway (`/webhooks/*`) mit HMAC.
- Event Contracts: Kafka Topics (`agent.events`, `pipeline.jobs`).
- Data Contracts: JSON Schemas fuer Tasks, Results, Ledger.
- Auth: OAuth2 Scopes (`tenant:read`, `tenant:write`, `ops:*`).

## Deliverables
- Architektur Diagramme inkl. Sequence Flows (Gateway ? Orchestrator ? Adapter).
- API Specs & SDKs (TS, Python, .NET), CLI Tools.
- IaC Module (Terraform/Bicep) fuer Core Services.
- Runbooks fuer Incident Response, Rollback, Tenant Onboarding.

## Offene Punkte & Abhaengigkeiten
- Auswahl finaler Event Bus (RabbitMQ vs Kafka) und Managed Optionen.
- Proof-of-Concept fuer Neon Read Replicas in produktionsnahen Regionen.
- Finale Definition Data Residency (EU vs Global) mit Legal/Compliance.
- Integration von Health Monitor Auto-Remediation Hooks.

## Links
- [Data Layer](md.html?path=data/data.md)
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Security Framework](md.html?path=security/security.md)
- [SLO Framework](md.html?path=slo/slo.md)
