# Plattform APIs

## Zweck & Scope
Dokumentiert exponierte APIs fuer Gateway, Orchestrator, Webhooks und Integrationen. Fokus auf Auth, Rate Limits und Vertragliche Garantien.

## Enterprise-Anforderungen
- OAuth2/OIDC Auth mit Scopes, Tenant Boundaries, Audit Logging.
- Rate Limits, Quotas und Budget Enforcement pro Agent/Tenant.
- Versionierung und Deprecation Policies (Semantic Versioning).
- SLA/SLO Definitionen fuer Antwortzeiten und Fehlerquoten.
- Security Kontrollen (mTLS, HMAC Signaturen, Input Validation).

## Technologie-Stack
- API Gateway basierend auf Traefik/Envoy mit OPA/Cerbos Policies.
- REST/gRPC Services (TypeScript, Go, Python) fuer Agents und Integrationen.
- Webhooks fuer ADO/GitHub, Slack/Teams, Incident Bots.
- Schema Definition via OpenAPI/AsyncAPI, JSON Schema.
- Observability: Prometheus, Jaeger/Tempo, Loki Logging.

## Architektur & Betrieb
APIs hinter Traefik Ingress, WAF Rules, DDoS Protection. Multi-tenant Routing ueber JWT Claims, Redis Rate Limiters. Canary Deployments pruefen neue Versionen. Health Monitor ueberwacht Heartbeats. Audit Logs landen in Neon.

## Schnittstellen & Vertraege
- REST: `/api/v1/tasks`, `/api/v1/agents`, `/api/v1/events` (JSON, HAL, ETag).
- gRPC: `AgentCoordinator`, `TaskQueue`, `LLMRouter` Services.
- Webhooks: `POST /webhooks/ado`, `POST /webhooks/github`, signiert.
- Rate Limit: `X-RateLimit-*` Header, Quota dashboards in Grafana.

## Deliverables
- OpenAPI/AsyncAPI Specs, Clients (TS, Python, .NET), SDK Docs.
- Example Requests/Responses, Postman Collection, Integration Tests.
- SLA Dokumentation, Incident Playbooks fuer API Outages.
- Versioning & Deprecation Plan, Release Notes.

## Offene Punkte & Abhaengigkeiten
- Automatisierte Contract Testing Pipeline (Pact, Dredd).
- Integration mit Security fuer API Hardening (WAF rules, fuzzing).
- Alignment mit Governance fuer External Partner Access.
- Abhaengigkeit zu Monitoring fuer Rate Limit KPIs.

## Links
- [Core Plattform](md.html?path=core/core.md)
- [Integration Azure DevOps](md.html?path=integration/azure-devops.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [Risk Register](md.html?path=risk/risk.md)
