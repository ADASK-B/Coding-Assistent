# KI-Entwicklungsbetrieb – Plattformhandbuch (Markdown, vollständig)

*Autor:* Solution Architecture Team
*Stand:* 2025-09-14
*Status:* Zielarchitektur & Aufbauplan (von Grund auf)

## 1) Ziel in einem Satz

Eine **KI-Entwicklungsplattform** auf Basis **Azure DevOps** (ADO), die mit **Agenten** wie echte Entwickler arbeitet (Tickets → Branch → Draft-PR → Build/Tests → Review → Merge), **multi-tenant** läuft, **sicher** ist, **beobachtbar** bleibt und **kostenbewusst** skaliert.

## 2) Mission & Leitprinzipien

* **Produktivität:** Kleine, geprüfte Änderungen in Minuten statt Stunden.
* **Sicherheit & Governance:** Jede Aktion ist nachvollziehbar, rückholbar und durch Richtlinien geschützt.
* **Skalierung:** Entkoppelt, ereignis- und warteschlangenbasiert, horizontal erweiterbar.
* **Kostenkontrolle:** Kleines Modell standard, großes nur bei Bedarf; Caching; Budgets.
* **Vendor-neutral:** Eigene Orchestrierung; mehrere LLM-Provider; austauschbare Bausteine.
* **Einfach erklärbar:** Begriffe kurz, Entscheidungen dokumentiert (ADRs).

## 3) Umfang (Scope)

### In Scope

* ADO-first (Boards/Repos/Pipelines), GitHub/GitLab via Adapter.
* Agenten agieren wie Entwickler: Branch, Draft-PR, Pipeline, Tests, Doku.
* Multi-Agent-Zusammenarbeit mit Koordinator und gemeinsamen Verträgen (Schemas).
* Mehrere LLMs: self-hosted (vLLM/TGI/Ollama) + Cloud (Azure/OpenAI/Anthropic/Google) über **Model-Router**.
* Multi-Tenant-Betrieb: Trennung auf Namespace/Netz/DB-Schema.
* Observability/FinOps/Security by design.

### Out of Scope (erste Ausbaustufe)

* Vollautonome Produktionseinsätze in Hochrisiko-Repos ohne Gate.
* Business-Entscheidungen außerhalb der Dev-Domäne (nur Zuarbeit/Analysen).

## 4) End-to-End-Ablauf (einfach)

1. **Ticket** auf dem ADO-Board (Akzeptanzkriterien vorhanden).
2. **PO-Agent** präzisiert Kriterien (kurz).
3. **Dev-Agent** holt Kontext, erstellt **Branch + Draft-PR** (kleiner Diff).
4. **Pipeline-Agent** erzeugt/aktualisiert **CI-YAML**.
5. **Test-Agent** startet Build/Tests, fixt zielgerichtet in wenigen Iterationen.
6. **Reviewer-Agent** kommentiert (Lint/Security/Policies).
7. **Merge** via Gate/Mensch; **Docs-Generator** aktualisiert README/CHANGELOG/ADR.

## 5) Autonomie-Modell & Risikoklassen

**Modi:** Vorschlag · Entwickler · Stabilisierung („bis grün“) · Koordination (Multi-Repo) · Release (nur Low-Risk).

**Risikoklassen:**

* **R0** Doku/Lint/Testergänzung → automatisierbar.
* **R1** kleiner Code-Diff/Minor-Upgrade → automatisierbar mit Logs.
* **R2** größere Änderungen/Multi-Repo → Gate/Review.
* **R3** kritisch (Security/Prod-nah) → verpflichtende manuelle Freigabe.

**Prinzip:** Frei arbeiten, aber **sichtbar** (Artefakte, Logs) und **rückholbar** (Rollback).

## 6) SLO/SLI-Ziele (Betrieb)

* **Verfügbarkeit (30d):** 99,9 %.
* **P95 Latenzen:**

  * Webhook → Annahme (202): **< 200 ms**
  * Dispatch Orchestrator: **< 1 s**
  * Agent-Einzelschritt (Default **≤ 60 s**, bei Bedarf bis **≤ 180 s** mit Approval)
  * Ticket → Draft-PR (kleiner Scope): **< 5 min**
* **Skalierung:** ≥ 100 Tenants, ≥ 1 000 parallele Executions.
* **Mandantentrennung:** 100 % (Namespaces/DB-Schema/Keys).
* **Kosten-Transparenz:** Ledger (Token/Runtime/Modell) je Tenant/Repo/Task.

## 7) Sicherheits- & Governance-Modell

* **Identität:** Service Principals pro Agentenrolle/Projekt (Least-Privilege, kurze Tokens).
* **Richtlinien:** OPA/Cerbos (RBAC/ABAC, Pfade/Zeilenlimits, Repo-Scopes).
* **Secrets:** Vault + External Secrets Operator; keine Klartexte in CI.
* **Supply-Chain:** SBOM (Syft), Scans (Trivy/Grype), Signaturen (Cosign), Policy-Gates.
* **Audit:** Unveränderliche Logs, Events (Kafka), vollständige Provenance.
* **Compliance:** GDPR/EU-AI-Act-ready (DPIA, Model Cards, Lineage, Human-in-the-Loop).

### 7.1) EU-AI-Act & DPIA (operationalisiert)

* **Use-Case-Klassifizierung:** Einstufung je Anwendungsfall (z. B. General-Purpose/High-Risk) inkl. Verantwortlichkeiten (Produkt/Compliance/Security).
* **DPIA (Datenschutz-Folgenabschätzung):** Datenflüsse (Prompt/Kontext/Logs) dokumentiert; **PII-Minimierung**; **Retention** je Datenklasse (z. B. Logs 30/90 Tage, Traces 14/30 Tage – projektabhängig).
* **Provider-Boundary:** Cloud-LLMs nur über Router; **No-Training-/Data-Boundary-Flags** setzen; egress-Allowlist.
* **Model Cards & Evaluation Reports:** Pflicht vor Release (Accuracy/Latency/Safety, Limits); Versionierung je Modell/Prompt.
* **Human-in-the-Loop:** Nachweisbar je Risikoklasse (R2/R3 Gate-Kriterien).
* **Policy-Guards:** Prompt-Injection-Filter (RAG-Filter, Tool-Allowlists), Output-Moderation (Security/Compliance-Checks).

## 8) Multi-Tenant-Design

* **K8s-Namespaces** pro Tenant/Projekt mit ResourceQuotas & NetworkPolicies.
* **DB-Schema pro Tenant**, KMS-Schlüssel pro Tenant.
* **Observability & Kosten** segmentiert je Tenant (Dashboards, Budgets, Alarme).

## 9) Systemkomponenten (Überblick)

* **Control-Plane:** n8n (Ingress/Approvals), **Agent-Gateway (MCP-Hub)**, **Capabilities-Registry**.
* **Orchestrierung:** Orchestrator (Temporal/Durable), **Agent-Coordinator**, Queues (RabbitMQ/Kafka).
* **Dev-Services:** Adapter (ADO/GH/GL), Repo-Ops, Pipeline-Factory, Docs-Generator, Sandbox-Runs.
* **KI/LLM:** Model-Router, vLLM/TGI, Ollama (Dev), Cloud-LLMs, MLflow (Registry).
* **Daten/Wissen:** PostgreSQL-HA, Redis, MinIO/S3, Vector-DB, Kosten-Ledger.
* **Security:** Entra/Keycloak, OPA/Cerbos, Vault/ESO, WAF/Front Door, Supply-Chain.
* **Observability:** OTel, Prometheus→Thanos, Grafana, Loki/Promtail, Tempo/Jaeger, Alertmanager.
* **Plattform:** Kubernetes, Helm/Kustomize, Argo CD, Service Mesh, Ingress, cert-manager, GPU-Operator.

## 10) Teams & Verantwortlichkeit (Squads + Architekten)

| Team                       | Auftrag                                                   | Kernrollen       | Haupt-KPIs                   |
| -------------------------- | --------------------------------------------------------- | ---------------- | ---------------------------- |
| Platform & Delivery        | K8s, GitOps, Ingress/Mesh, Runner                         | SRE/Platform/IaC | Verfügbarkeit, MTTR, Drift=0 |
| Control-Plane              | Event-Ingress, Approvals, Routing (n8n)                   | Backend/Low-Code | P95 Ingest < 200 ms          |
| Orchestrator & Workflows   | Workflows/State/Retries/Idempotenz                        | Backend/Workflow | P95 Dispatch < 1 s           |
| Adapters & Repo-Ops        | ADO/GH/GL APIs, Branch/Diff/PR                            | Backend          | PR-Durchsatz                 |
| Pipeline-Factory & Runners | Templates, Runner-Pools, Gates                            | DevOps           | Build-Grün-Rate              |
| LLM Platform & Router      | Routing, vLLM/TGI/Ollama                                  | ML/MLOps         | Kosten/Antwort               |
| Data & Retrieval           | PG/Redis/MinIO/Vector, Indexer, MLflow                    | Data/DB          | Query-Latenz                 |
| Security & Compliance      | SSO/RBAC, OPA, Vault, Supply-Chain                        | Sec/Compliance   | Verstöße=0                   |
| Observability & FinOps     | OTel/Prom/Graf/Loki/Tempo, Ledger                         | SRE/FinOps       | Kosten/PR                    |
| Product Enablement         | Project-Factory, Team-Directory, Templates, Admin-Console | Platform/Backend | TTM neues Projekt            |

**Architekten (RACI):** Enterprise Architect (A/R), Solution Architect (A/R), Security Architect (C/R), Data/ML Architect (C/R), FinOps (A/R), DPO/Compliance (C), CAB (A/C).

## 11) Repositories – Plan & Inhalte

**Namensschema:** `svc-*` (Services), `app-*` (UIs), `infra-*` (Infrastruktur), `policy-*`, `chart-*`, `ops-*`, `lib-*`, `contract-*`.

### 11.1 Repos (Auszug, API-first)

**Core & Control**

* `svc-gateway` – API-Gateway (AuthZ/Rate/Router).
* `svc-control-n8n` – n8n-Flows as Code (Ingress, Approvals, Benachrichtigungen).
* `svc-agent-gateway` – MCP-Hub (Agent-„Steckdose“, Routing, Quotas, Audit).
* `svc-capabilities-registry` – Katalog der Tools/Skills (Policies/Kosten).
* `svc-agent-coordinator` – Multi-Agent-Plan/Übergaben/Checks (Task-Graph).
* `svc-sandbox` – Ephemere Fork/Env, Pre-Flight-Checks (TTL).

**Orchestrierung & Dev-Services**

* `svc-orchestrator` – Workflows/Workers (State/Retries/Idempotenz/DLQ).
* `svc-adapter-ado` – Boards/Repos/Pipelines (ADO REST).
* `svc-repo-ops` – Branch/Diff/PR/CODEOWNERS.
* `svc-pipeline-factory` – YAML-Render/Policy-Checks.
* `svc-docs-generator` – README/ADR/Changelog.

**KI/LLM & Retrieval**

* `svc-model-router`, `svc-llm-serving` (vLLM/TGI Manifeste), `svc-indexer` (Embeddings/RAG), `svc-cost-ledger` (Kosten & Admission).

**Infra/Policies/Obs**

* `infra-iac`, `infra-gitops`, `chart-*`, `policy-opa`, `policy-supplychain`, `ops-observability`, `ops-db-migrations`.

**Libraries & Contracts**

* `lib-sdk-ts/py/dotnet` (Clients), `contract-openapi`, `contract-events`, `contract-task` (Task/Result-Schemas), `templates-pipelines`, `templates-repo`.

**Hinweis:** **ADO MCP Server** wird über den MCP-Hub angebunden (Release-Status je Einsatzumgebung vermerken).

### 11.2 Repo-Content-Map (konkret)

*(Auszug – Beispielstruktur je Kern-Repo)*

| Repo                      | Sprache/Framework   | API/Schema              | Hauptfunktionen                                   | Schlüsselverzeichnisse              | Abhängigkeiten            | Secrets/Config              | CI-Schritte                          |
| ------------------------- | ------------------- | ----------------------- | ------------------------------------------------- | ----------------------------------- | ------------------------- | --------------------------- | ------------------------------------ |
| svc-agent-gateway         | TS/Node (Fastify)   | OpenAPI+WS; MCP-Brücke  | MCP-Hub, Token-Exchange, Routing, Quotas, Audit   | `/src` `/api` `/policies` `/tests`  | PG, Redis, OIDC, ADO MCP  | OIDC Issuer, DB URL, Limits | Lint→Unit→Contract→Image→Helm→Deploy |
| svc-capabilities-registry | TS/Node             | OpenAPI                 | Tools/Skills verwalten, Discover, Policies/Kosten | `/src` `/schemas` `/tests`          | PG                        | DB URL                      | Lint→Unit→Contract→Image→Deploy      |
| svc-agent-coordinator     | TS/Node             | OpenAPI                 | Plan/Task-Graph/Übergaben/Checks                  | `/src` `/workflows` `/tests`        | Orchestrator SDK, PG      | Orchestrator URL, DB        | Lint→Unit→Contract→Image→Deploy      |
| svc-sandbox               | TS/Node             | OpenAPI                 | Ephemere Fork/Branch/Env, Pre-Flight, Cleanup     | `/src` `/adapters` `/tests`         | Repos/Pipelines APIs      | ADO Conn, TTL               | Lint→Unit→Contract→Image→Deploy      |
| contract-task             | JSON/YAML           | JSON-Schema             | Einheitl. Auftrags/Ergebnis-Formate               | `/schemas` `/examples`              | –                         | –                           | Schema-Validate→Publish              |
| svc-orchestrator          | TS (Temporal SDK)   | gRPC/Worker             | Workflows/Activities, DLQ, Idempotenz             | `/workflows` `/activities` `/tests` | Temporal/PG               | Conn/Queues                 | Unit→Workflow-Tests→Image→Deploy     |
| svc-adapter-ado           | TS/Node             | OpenAPI                 | Boards/Repos/Pipelines CRUD                       | `/src` `/clients` `/tests`          | ADO REST, PG              | ADO App Reg., DB            | Lint→Unit→Contract→Image→Deploy      |
| svc-repo-ops              | TS/Node             | OpenAPI                 | Branch/Diff/PR/CODEOWNERS                         | `/src` `/git` `/tests`              | Git, Adapter              | SCM Tokens                  | Lint→Unit→Contract→Image→Deploy      |
| svc-pipeline-factory      | TS/Node             | OpenAPI                 | YAML-Render/Checks/Templates                      | `/src` `/templates` `/tests`        | ADO, Vault                | Keys/Registries             | Lint→Unit→Contract→Image→Deploy      |
| svc-docs-generator        | Python/FastAPI      | OpenAPI                 | README/ADR/Changelog                              | `/app` `/templates` `/tests`        | Jinja2/Git                | –                           | Flake8→Pytests→Image→Deploy          |
| svc-model-router          | Python/FastAPI      | OpenAI-Compat + OpenAPI | Modellwahl/Budget/Audit/Caching                   | `/app` `/providers` `/tests`        | vLLM/Cloud LLMs, PG/Redis | Provider Keys               | Lint→Unit→Contract→Image→Deploy      |
| svc-llm-serving           | K8s/Helm            | –                       | vLLM/TGI Deployments, Autoscaling                 | `/manifests` `/helm`                | GPU Operator              | Images/Models               | Helm Lint→Deploy                     |
| svc-indexer               | Python/FastAPI      | OpenAPI                 | Embeddings/Summaries, Vektor-Writes               | `/app` `/workers` `/tests`          | VecDB, Router, PG         | URLs                        | Pytests→Image→Deploy                 |
| svc-cost-ledger           | TS/Node             | OpenAPI                 | Kostenbuch & Admission                            | `/src` `/rules` `/tests`            | Prom/Router/PG            | DB, Thresholds              | Lint→Unit→Contract→Image→Deploy      |
| infra-iac                 | Terraform/Bicep     | –                       | Cluster/Netz/Vault/DB                             | `/envs` `/modules`                  | Cloud APIs                | SPNs/Keys                   | fmt→validate→plan→apply              |
| infra-gitops              | Helm/Kustomize/Argo | –                       | App-of-Apps, Environments                         | `/apps` `/envs`                     | Git/K8s                   | Argo Conn                   | Helm Lint→Sync                       |
| chart-\*                  | Helm                | –                       | Charts je Service                                 | `/charts/<svc>`                     | –                         | –                           | Helm Lint→Publish                    |
| policy-opa                | Rego                | –                       | RBAC/ABAC/Path/LOC Limits                         | `/policies` `/tests`                | OPA                       | –                           | Conftest→Publish                     |
| policy-supplychain        | YAML/JSON           | –                       | SBOM/Trivy/Cosign Regeln                          | `/rules`                            | –                         | –                           | Pipeline-Check                       |
| ops-observability         | Helm/Kustomize      | –                       | OTel/Prom/Graf/Loki/Tempo/Alert                   | `/manifests`                        | –                         | –                           | Lint→Deploy                          |
| ops-db-migrations         | SQL/Flyway          | –                       | PG Schema/Tenant-Trennung                         | `/migrations`                       | PG                        | DB URL                      | Validate→Migrate                     |
| app-admin-console         | React/Next.js       | OpenAPI-Clients         | UI Projekte/Teams/Budgets/Scorecards              | `/src`                              | Gateway APIs              | OIDC                        | Lint→Unit→Build→Deploy               |
| lib-sdk-ts/py/dotnet      | TS/Python/.NET      | OpenAPI Clients         | SDKs für Gateway/Orchestrator                     | `/src`                              | –                         | –                           | Build→Test→Publish                   |

**Standard je Repo:** OpenAPI/Schema, Dockerfile, Helm-Chart, CI-Pipeline, OTel-Instrumentierung, README + **ADR-0**.
**API-Verbrauch (ADO):** Rate-Limits beachten (exponentielles Backoff mit Jitter), **Idempotency Keys** für write-Ops, Hook-Retry-Pfade.

## 12) Projekt-Factory & Team-Directory

**Projekt-Factory:** ADO-Projekt mit Boards/Repos/Pipelines/Policies/Teams/Wiki auf Knopfdruck (idempotent, \~10 min).
**Team-Directory:** Zentrale YAML/JSON → ADO-Gruppen, **CODEOWNERS**, Teams/Slack-Kanäle.

## 13) LLM-Schicht (Modell-Router & Serving)

* **Router:** wählt Modell nach Kosten/Latenz/Datenschutz; redigiert PII; führt Budgets/Quotas.
* **Self-hosted:** vLLM/TGI (GPU-Pools, Auto-Scaling, KV-Cache).
* **Dev lokal:** Ollama (kleine Modelle).
* **Cloud:** Azure/OpenAI/Anthropic/Google via Router (Policies/Boundary).

### 13.1) LLM-Evaluation & Safety-Gates

* **Golden-Sets & Regression:** gepflegte Prompt-/Task-Suiten je Agent-Skill; automatisierte Regression.
* **Gates (vor Merge/Release):** Max-Kosten/Antwort, P95-Latenz, Safety-Score (Toxicity/Bias) ≥ Schwelle.
* **Prompt-Versionierung:** SemVer + ADR + Changelog; Tests bei Änderungen.
* **Canary-Routing:** z. B. 10 % Traffic; Auto-Rollback bei SLO-/Kostenverletzung.
* **Tool-Allowlists & Guardrails:** erlaubte Tools pro Tenant/Repo; Output-Moderation als letzter Schritt.

## 14) Daten- & Wissensebene

* **PostgreSQL-HA:** zentrale Transaktions-DB (Mandanten-Schemata, Metriken, Audit).
* **Redis (Cluster):** Cache/Rate/Idempotenz, leichte Jobs.
* **MinIO/S3:** Artefakte/Logs/Modelle (Versioning, Object-Lock).
* **Vector-DB:** RAG/Kontext (pgvector/Weaviate/Qdrant).
* **MLflow:** Modell-Registry (Versionen, Freigaben).
* **Kosten-Ledger:** Tokens/Runtime/Modellkosten je Task/Team; Admission Control.

## 15) Control-Plane (Events & Approvals)

* **n8n (Queue-Mode):** Webhook-Ingress, Genehmigungen, Benachrichtigungen (Teams/Slack/Email). **Redis als zentrale Queue/Rate/State**; getrennte Deployments: *Main*, *Webhook*, *Worker*. **Kein lokales FS** für Binaries (Container-Volumen/Objektspeicher nutzen).
* **Agent-Gateway (MCP-Hub):** einheitlicher Anschluss für **interne/externe Agenten**, inkl. ADO-MCP-Server.
* **Capabilities-Registry:** Tool/Skill-Katalog (finden, prüfen, aufrufen).
* **Agent-Coordinator:** Multi-Agent-Task-Graph (Plan, Zuweisung, Prüfung, Synthese).
* **Sandbox-Runs:** sichere Vorstufe (Fork/Env) vor echtem PR.

## 16) Observability & FinOps

* **OTel** überall; **Prometheus→Thanos**, **Grafana**, **Loki**, **Tempo/Jaeger**, **Alertmanager**.
* **Dashboards:** SLOs, Kosten/PR/Team, Cache-Hit-Rate, Fehlerklassen.
* **Budgets & Alarme:** 80/90/100 %-Schwellen; **Admission Control** bei Überlast/Überbudget.
* **SIEM-Anschluss:** Weiterleitung relevanter Logs/Traces/Metriken (z. B. an Unternehmens-SIEM) für Korrelation & Alarme.
* **FinOps konkret:** Budgets je Tenant/Repo/Task; Spot-/Preemptible-GPU-Pools für non-critical Inferenz; **KV-Cache-Hit-Rate** als KPI; skalieren über **Karpenter (Azure Provider)** bzw. Cluster Autoscaler je Workload-Pool.

## 17) Inbetriebnahme (Startreihenfolge)

1. **Infrastruktur:** `infra-iac` (Cluster/Netz/Vault/DB/Redis/MinIO).
2. **GitOps:** `infra-gitops` (Argo App-of-Apps) → alle `chart-*`.
3. **Basis:** Gateway, Orchestrator, Adapter, Redis, PG, n8n, Observability.
4. **Funktionen:** Repo-Ops, Pipeline-Factory, Docs-Generator, Indexer, Model-Router, LLM-Serving.
5. **Enablement:** Project-Factory, Team-Directory, Admin-Console.
6. **ADO Hooks:** ADO → n8n → Orchestrator.
7. **Policies/Gates:** OPA/Supply-Chain; Branch-Policies; Kosten-Ledger aktiv.

### 17.1) DR/BCP & Incident Handling

* **RTO/RPO je Datenklasse:**

  * PG: RTO ≤ 30 min / RPO ≤ 5 min (WAL-Archival, Multi-Zone).
  * MinIO: RTO ≤ 60 min / RPO ≤ 15 min (Versioning, Object-Lock).
  * Vector-DB & MLflow: RTO ≤ 60 min / RPO ≤ 15 min.
* **Backups & Restore-Drills:** quartalsweise Probedurchlauf (Skripte + Runbook).
* **DLQ-Replay:** batched Replays mit Backoff; Idempotency Keys verpflichtend.
* **Break-Glass:** TOTP-geschützte, zeitlich begrenzte Admin-Accounts (≤ 1 h), vollständiger Audit-Trail.
* **Secrets-Leak Pfad:** Fund → Key-Revoke → Secret-Rotation → Forensik/Tickets → Lessons Learned (ADR).
* **Supply-Chain Gates:** CVSS-Schwellen, Cosign-Verify, SBOM-Delta-Checks vor Deploy.

## 18) 90-Tage-Aufbau (Pilot → produktiv)

* **Woche 1–2:** Gateway, Orchestrator-Skelett, n8n-Ingress, Observability „Hello-World“.
* **Woche 3–4:** Adapter-ADO, Repo-Ops, Pipeline-Factory; ADO-Service-Hooks live.
* **Woche 5–6:** Model-Router + vLLM-Pool; Indexer + Vector-DB; Docs-Generator; Kosten-Ledger.
* **Woche 7–8:** Agent-Gateway (MCP-Hub), Capabilities-Registry, Agent-Coordinator (MVP), Sandbox-Runs.
* **Woche 9–12:** Project-Factory, Team-Directory, Policies komplett; Pilot in 2–3 Projekten (Shadow → Gates).

## 19) Mermaid-Diagramme

### 19.1) Landschaft (L0, Gesamtübersicht)

```
flowchart TB
  linkStyle default stroke:#888,stroke-width:1.2px
  classDef core fill:#e8f5e9,stroke:#2e7d32,color:#1b5e20
  classDef control fill:#e3f2fd,stroke:#1565c0,color:#0d47a1
  classDef orchestrator fill:#ede7f6,stroke:#5e35b1,color:#311b92
  classDef ai fill:#fff8e1,stroke:#f9a825,color:#e65100
  classDef data fill:#f3e5f5,stroke:#6a1b9a,color:#4a148c
  classDef obs fill:#e0f7fa,stroke:#00838f,color:#004d40
  classDef sec fill:#ffebee,stroke:#c62828,color:#b71c1c
  classDef plat fill:#f5f5f5,stroke:#616161,color:#212121
  classDef msg fill:#fff3e0,stroke:#ef6c00,color:#e65100
  classDef dev fill:#e0f2f1,stroke:#00796b,color:#004d40

  subgraph USERS[👤 Nutzer & Identität]
    U[Benutzer]:::dev
    IDP[Entra ID / Keycloak\n(SSO/Rollen)]:::sec
  end
  U --> IDP

  subgraph ADO[Azure DevOps (SaaS)]
    Boards[Boards (Work Items)]:::core
    Repos[Repos (Git)]:::core
    Pipelines[Pipelines (Build/Test/Deploy)]:::core
    Hooks[Service Hooks / Webhooks]:::control
    ACR[Container Registry]:::core
    Artifacts[Artifacts/NuGet/npm]:::core
    MCP[ADO MCP Server]:::control
  end
  IDP -. Login .-> ADO
  U -->|arbeitet mit| Boards
  U -->|Commit/PR| Repos

  WAF[WAF / Front Door]:::sec
  IDP --> WAF

  subgraph K8S[Kubernetes-Plattform]
    direction TB
    Ingress[Ingress (Traefik/Nginx)]:::plat
    Mesh[Service Mesh (Istio/Linkerd)]:::plat
    Cert[cert-manager]:::plat
    GPUOp[GPU Operator/Karpenter]:::plat
    Argo[Argo CD (GitOps)]:::plat

    subgraph CTRL[Control-Plane]
      direction TB
      N8Main[n8n Main/UI]:::control
      N8Hook[n8n Webhook]:::control
      N8Work[n8n Worker]:::control
      RedisQ[Redis (Queue/Rate/Cache)]:::data
      Teams[Teams/Slack/Email]:::control
      Gateway[Gateway/API (AuthZ/Rate)]:::core
      AgentGW[Agent-Gateway (MCP-Hub)]:::control
      CapReg[Capabilities-Registry]:::control
    end

    subgraph ORCH[Orchestrator & Agent-Dienste]
      direction TB
      Orchestrator[Temporal / Azure Durable\n(State/Retries/Timers)]:::orchestrator
      QueueMQ[RabbitMQ / Kafka (Jobs/Events)]:::msg
      Adapter[Adapter (ADO/GitHub/GitLab APIs)]:::core
      RepoOps[Repo-Ops (Branch/Diff/PR/Labels)]:::core
      PipeFactory[Pipeline-Factory (YAML-Vorlagen)]:::core
      DocsGen[Docs-Generator (README/ADR/Changelog)]:::core
      POAgent[PO-Agent]:::orchestrator
      DevAgent[Dev-Agent]:::orchestrator
      TestAgent[Test-Agent]:::orchestrator
      RevAgent[Reviewer/Governance-Agent]:::orchestrator
      Indexer[Indexer (Embeddings/Summaries)]:::core
      ACoord[Agent-Coordinator\n(Planner/Allocator/Arbiter)]:::orchestrator
      Sandbox[Sandbox-Runs (Fork/Env)]:::core
    end

    subgraph EXT[Externe Agenten]
      GHAgent[GitHub/GitLab/Atlassian Agenten]:::control
      OtherAgents[Andere Agent-Dienste]:::control
    end

    subgraph AI[KI-Schicht]
      direction TB
      Router[Model-Router/Gateway\n(Policies/Budgets)]:::ai
      vLLM[vLLM / TGI (Self-hosted Serving)]:::ai
      Ollama[Ollama (Lokal/Dev)]:::ai
      CloudLLM[Cloud-LLMs (Azure/OpenAI/Anthropic/Google)]:::ai
      MLflow[Model-Registry (MLflow)]:::data
    end

    subgraph DATA[Daten & Wissen]
      direction TB
      PG[PostgreSQL-HA (Multi-Tenant)]:::data
      Redis[Redis Cluster (Cache/Idempotenz)]:::data
      MinIO[MinIO / S3 (Artefakte/Logs/Models)]:::data
      VecDB[Vektor-DB (pgvector/Weaviate/Qdrant)]:::data
      Cost[Kosten-Ledger / Admission Control]:::data
      TaskContract[Task-Contract Schemas]:::data
    end

    subgraph OBS[Beobachtung]
      direction TB
      OTel[OpenTelemetry Collector]:::obs
      Prom[Prometheus → Thanos]:::obs
      Graf[Grafana (Dashboards)]:::obs
      Loki[Loki + Promtail (Logs)]:::obs
      Tempo[Tempo/Jaeger (Traces)]:::obs
      Alert[Alertmanager]:::obs
      Health[Health Monitor (SLA/SLO)]:::obs
    end

    subgraph SEC[Sicherheit & Governance]
      direction TB
      OPA[OPA/Cerbos (Policies as Code)]:::sec
      Vault[Vault + External Secrets Operator]:::sec
      Supply[Supply Chain: Syft/Trivy/Cosign]:::sec
      SecretsScan[Secrets-Scanner (Gitleaks)]:::sec
      Quality[SonarQube/FOSSA (Qualität/Lizenzen)]:::sec
    end
  end

  Hooks --> MCP
  Hooks --> WAF --> Ingress --> N8Hook
  MCP --> AgentGW
  N8Hook --> RedisQ
  N8Work --> Gateway
  AgentGW --> Gateway
  CapReg --> AgentGW
  EXT --> AgentGW

  Orchestrator <--> QueueMQ
  Orchestrator --> Adapter
  Orchestrator --> RepoOps
  Orchestrator --> PipeFactory
  Orchestrator --> DocsGen
  Orchestrator --> POAgent
  Orchestrator --> DevAgent
  Orchestrator --> TestAgent
  Orchestrator --> RevAgent
  Orchestrator --> ACoord
  Orchestrator <--> PG
  Gateway <--> Redis
  Gateway --> OPA

  Adapter <--> Boards
  Adapter <--> Repos
  Adapter <--> Pipelines
  Pipelines --> ACR
  Pipelines --> Artifacts

  POAgent --> Router
  DevAgent --> Router
  TestAgent --> Router
  RevAgent --> Router
  Indexer --> VecDB
  Router <--> vLLM
  Router <--> CloudLLM
  Router <--> Ollama
  Router --> PG
  Router --> Cost
  DevAgent --> RepoOps
  TestAgent --> Pipelines
  RevAgent --> Quality
  ACoord --> Sandbox
  Sandbox --> Repos
  Sandbox --> Pipelines

  PipeFactory --> Pipelines
  PipeFactory --> Vault
  RepoOps --> Adapter
  DocsGen --> Repos

  N8Main <--> PG
  N8Main <--> Redis
  Adapter <--> PG
  RepoOps <--> PG
  PipeFactory <--> PG
  DocsGen <--> PG
  Indexer <--> PG
  vLLM <--> MLflow
  Router <--> MLflow

  Pipelines --> MinIO
  Loki --> MinIO
  TaskContract --> AgentGW
  TaskContract --> ACoord

  Gateway --> OTel
  Orchestrator --> OTel
  Adapter --> OTel
  RepoOps --> OTel
  PipeFactory --> OTel
  DocsGen --> OTel
  N8Main --> OTel
  N8Hook --> OTel
  N8Work --> OTel
  Router --> OTel
  vLLM --> OTel
  Ollama --> OTel
  Indexer --> OTel
  AgentGW --> OTel
  ACoord --> OTel
  Sandbox --> OTel
  Prom <--> OTel
  Loki <--> OTel
  Tempo <--> OTel
  Graf --> Prom
  Graf --> Loki
  Graf --> Tempo
  Prom --> Alert
  Prom --> Health
  Alert --> Teams

  IDP --> N8Main
  IDP --> Graf
  IDP --> Gateway
  Vault --> Orchestrator
  Vault --> Adapter
  Vault --> RepoOps
  Vault --> PipeFactory
  Vault --> vLLM
  Vault --> AgentGW
  Vault --> ACoord
  OPA --> Gateway
  OPA --> Orchestrator
  Supply --> Pipelines
  SecretsScan --> Repos
  Quality --> Pipelines

  Adapter --> QueueMQ
  QueueMQ --> OBS

  WAF --> Ingress
  Ingress --> Mesh
  Mesh --> Gateway
  Cert --> Ingress
  GPUOp --> vLLM
  Argo --> K8S

  subgraph DEV[Entwicklung zu Hause (lokal)]
    DevBox[Dev-Container/Compose\n(Gateway-Dev, Orchestrator-Light, PG, Redis, Ollama)]:::dev
    LocalGit[Git Client]:::dev
  end
  LocalGit --> Repos
  DevBox --> LocalGit
  DevBox --> Ollama
  DevBox --> PG
  DevBox --> Redis
```

### 19.2) Datenpfad (L1, vereinfacht)

```
flowchart LR
  classDef ai fill:#fff8e1,stroke:#f9a825,color:#e65100
  classDef data fill:#f3e5f5,stroke:#6a1b9a,color:#4a148c
  classDef ctrl fill:#e3f2fd,stroke:#1565c0,color:#0d47a1

  DevAgent:::ctrl -->|Task/Prompt| Router[Model-Router]:::ai
  Router -->|Policy/Budget| LLM{Self-hosted\nvLLM/TGI\noder Cloud}:::ai
  DevAgent -->|Kontext| Indexer:::ctrl
  Indexer --> VecDB[Vektor-DB]:::data
  Router -->|PII-Redaktion/Logs| PG[(PostgreSQL Ledger)]:::data
  LLM -->|Antwort| DevAgent
  DevAgent -->|Artefakte| Repos
```

## 20) Technologie-Stack – Tabellen (vollständig, einfach erklärt)

**Spalten:** *Technologie/Service · Funktion · Verbindungen · Betriebsmodus/HA · Daten/Speicher · Sicherheit/Compliance · Kostenhebel/Sizing · Alternativen*

### 20.1 Core-Dienste

| Technologie/Service             | Funktion                   | Verbindungen                       | Betriebsmodus/HA | Daten/Speicher   | Sicherheit/Compliance  | Kostenhebel/Sizing   | Alternativen   |
| ------------------------------- | -------------------------- | ---------------------------------- | ---------------- | ---------------- | ---------------------- | -------------------- | -------------- |
| Gateway (API)                   | AuthZ, Rate, Routing       | Ingress, Orchestrator, Redis, OTel | 2+ Repl., HPA    | Konfig in Git    | OIDC, mTLS             | Horizontal skalieren | Kong/NGINX     |
| Orchestrator (Temporal/Durable) | Workflows/State/Retries    | Gateway, Queues, Adapter           | HA-Cluster       | Orchestrator-DB  | RBAC, NetPolicies      | Worker-Pool          | Argo/Prefect   |
| Adapter (ADO/GH/GL)             | Boards/Repos/Pipelines API | ADO/GH/GL                          | 2+ Repl.         | –                | OAuth, Least-Privilege | leicht               | SDKs           |
| Repo-Ops                        | Branch/PR/Diff/CODEOWNERS  | Adapter                            | Stateless        | –                | Commit-Sign/Policies   | gering               | Git-CLI        |
| Pipeline-Factory                | CI-YAML + Checks           | ADO, Vault                         | Stateless        | Templates in Git | Policy-Checks          | minimal              | Tekton/Jenkins |
| Docs-Generator                  | README/ADR/Changelog       | Repo-Ops                           | Stateless        | Artefakt → Repo  | Lizenz-Check           | gering               | DocFX/Sphinx   |
| Agent-Coordinator               | Multi-Agent-Plan & Checks  | Orchestrator, Sandbox, Registry    | HA               | PG               | OIDC/Scopes            | moderat              | –              |
| Sandbox-Runs                    | Fork/Env/Pre-Flight        | Repos, Pipelines                   | Isoliert         | Temp-Artefakte   | RBAC/Isolation         | TTL                  | –              |

### 20.2 DevOps/SCM

| Technologie/Service        | Funktion           | Verbindungen   | Betriebsmodus/HA | Daten/Speicher | Sicherheit      | Kosten        | Alternativen |
| -------------------------- | ------------------ | -------------- | ---------------- | -------------- | --------------- | ------------- | ------------ |
| ADO Boards/Repos/Pipelines | Work, Code, CI/CD  | Adapter, Hooks | SaaS             | SaaS           | Entra ID        | Lizenz/Runner | Jira/GH/GL   |
| ADO Hooks/MCP              | Events/ADO-Kontext | n8n/Agent-GW   | SaaS             | –              | Signierte Hooks | –             | GH/GL Hooks  |
| Registry (ACR/GHCR)        | Container-Images   | Pipelines, K8s | Geo-HA           | Blob           | Cosign          | Storage-Tier  | Harbor/ECR   |

### 20.3 Control-Plane

| Technologie/Service     | Funktion              | Verbindungen                  | Betriebsmodus/HA                | Daten/Speicher      | Sicherheit      | Kosten        | Alternativen    |
| ----------------------- | --------------------- | ----------------------------- | ------------------------------- | ------------------- | --------------- | ------------- | --------------- |
| n8n (Queue-Mode)        | Ingress/Approvals     | Hooks, Queues, Teams          | Main/Webhook/Worker + **Redis** | Postgres, **Redis** | SSO/RBAC, Audit | Worker-Anzahl | Power Automate  |
| Agent-Gateway (MCP-Hub) | Anschluss Agenten/KIs | MCP, externe Agenten, Gateway | 2+ Repl.                        | PG (Audit)          | OIDC, Quotas    | Durchsatz     | Mediator        |
| Capabilities-Registry   | Tool/Skill-Katalog    | Agent-GW, Orchestrator        | HA                              | PG                  | RBAC            | Cache         | Service Catalog |

### 20.4 LLM-Schicht

| Technologie/Service | Funktion                 | Verbindungen         | Betriebsmodus/HA | Daten/Speicher | Sicherheit    | Kosten          | Alternativen           |
| ------------------- | ------------------------ | -------------------- | ---------------- | -------------- | ------------- | --------------- | ---------------------- |
| Model-Router        | Auswahl/Policies/Budgets | vLLM/TGI, Cloud LLMs | 2+ Repl.         | Metadaten      | PII-Redaktion | Tiering/Caching | OpenRouter             |
| vLLM/TGI            | Serving self-hosted      | Router, GPU          | GPU-Pools        | KV-Cache       | mTLS, Quotas  | Quantisierung   | TensorRT-LLM           |
| Ollama              | Lokal/Dev                | Router               | Einzel           | Cache          | Dev-Creds     | Low-Cost        | LM Studio              |
| Cloud-LLMs          | Managed                  | Router               | SaaS             | SaaS           | Data-Boundary | Pay-per-use     | Azure/OpenAI/Anthropic |

### 20.5 Daten/Wissen

| Technologie/Service | Funktion                  | Verbindungen         | Betriebsmodus/HA | Daten/Speicher | Sicherheit        | Kosten          | Alternativen    |
| ------------------- | ------------------------- | -------------------- | ---------------- | -------------- | ----------------- | --------------- | --------------- |
| PostgreSQL-HA       | Transaktionen             | Core/Orch./Ledger    | HA, WAL-G        | SSD, PITR      | TDE/RBAC          | Right-Sizing    | Azure PG        |
| Redis Cluster       | Cache/Queue/Rate          | Gateway/n8n/Worker   | HA               | RAM            | TLS/ACL           | RAM-Größe       | Valkey          |
| MinIO/S3            | Artefakte/Logs/Modelle    | Pipelines, Logs      | Erasure/Version  | Object         | Object-Lock       | Storage-Klassen | Azure Blob      |
| Vector-DB           | RAG/Embeddings            | Indexer/LLM          | HA/Cluster       | DB/Cluster     | Access Controls   | Compression     | Milvus          |
| MLflow              | Modell-Registry           | Router/Serving       | HA               | DB+Objekt      | RBAC/Lineage      | Retention       | W\&B/DVC        |
| Kosten-Ledger       | Kosten/Quotas             | Router/Orch./Prom    | HA               | PG             | Audit             | Budgets         | Cloud Cost Mgmt |
| Task-Schemas        | Auftrags/Ergebnis-Schemas | Agent-GW/Coordinator | Git              | JSON/YAML      | Signierte Commits | –               | –               |

### 20.6 Observability

OTel, Prometheus→Thanos, Grafana, Loki/Promtail, Tempo/Jaeger, Alertmanager, Health Monitor, **SIEM-Anbindung**.

### 20.7 Security & Compliance

Entra/Keycloak (SSO), OPA/Cerbos, Vault/ESO, WAF, Supply-Chain (Syft/Trivy/Cosign), Gitleaks, SonarQube/FOSSA.

### 20.8 Messaging/Integration

Kafka (Events/Audit), RabbitMQ/Redis Streams (Jobs).

### 20.9 Plattform

Kubernetes (AKS/On-prem), Helm/Kustomize, Argo CD, Service Mesh, Ingress, cert-manager, GPU-Operator, **Karpenter (Azure Provider)**/**AKS Cluster Autoscaler**.

### 20.10 Developer Experience

Dev-Container/Compose, Make/Taskfile, Tests (Unit/Contract/E2E).

### 20.11 FinOps

Kosten-Ledger, Admission Control (Budgets/Rate/Quotas), Alarme, **Spot-/Preemptible-GPU-Pools**, KV-Cache-Hit-Rate.

## 21) Qualitäts- & Abnahme-Checklisten

**Architektur:** ADR vollständig; Datenklassen; SLO/DR; Security (Secrets, Threat, Supply-Chain); Observability; Rollback.
**PR:** Ticket verlinkt; kleiner, klarer Diff; Build/Tests/Lint/Sec grün; Doku/Changelog; Reviewer; keine Secrets; Kosten-Notiz.
**Limits & Backoff:** ADO-API 429 → exponentielles Backoff mit Jitter; Idempotency Keys; Hook-Retry → n8n/Orchestrator; DLQ-Replay definiert.

## 22) Beispiel-Szenarien

* **Feature (Backend+UI):** Ticket → Entwurf → Branch/Draft-PR → Build/Tests → Review/Gate → Merge → Doku.
* **Fehlende Pipeline:** Agent ergänzt YAML-Vorlage → PR → Build grün.
* **Minor-Upgrade:** Version anheben, Code anpassen, Tests grün, Changelog.
* **Secret-Leak:** Scanner schlägt an → Fix-Branch → Scans grün → Gate.
* **Multi-Repo:** Koordination → Contract-PRs → PR-Kaskade → gemeinsame Reviews.

## 23) Glossar (kurz & klar)

**Draft-PR**: Entwurf eines Pull Requests.
**Orchestrator**: Ablaufsteuerung mit Zustand/Retries.
**Queue**: Warteschlange für Jobs/Events.
**RAG/Vector-DB**: „Bedeutungssuche“ für Kontext.
**SSO**: Einmal anmelden, überall drin.
**Vault**: Tresor für Passwörter/Schlüssel.
**Policies as Code**: Regeln in Dateien, versioniert.
**SLO**: Zielwerte (z. B. Latenz/Verfügbarkeit).

**Dieses Dokument ist zur direkten Nutzung in Repo/Wiki geeignet.**
