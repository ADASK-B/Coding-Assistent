# Team-Aufgaben-Checklisten – nach Startreihenfolge

> Ziel: Jede Gruppe hat eine **abhakbare** Liste. Wenn alle Kästchen fertig sind → Bereich „work done“.

---

## Phase A1 – Platform & Delivery (Kubernetes)

**Auftrag:** Cluster & GitOps-Basis, Ingress/TLS, Node-/GPU-Pools, Autoscaling, Runbooks.

### Planung & Grundlagen

* [ ] Architektur-ADR für Plattform (AKS/On‑prem, Netztopologie, DNS, Sizing) erstellt
* [ ] Namens- & Label-Konvention (env, tenant, app, version, cost-center) definiert
* [ ] Cluster-SLOs festgelegt (Verfügbarkeit, Upgrade-Fenster, MTTR)
* [ ] Kapazitätsmodell v1 (System/Workload/GPU-Pools, Reserven) vorhanden

### Provisionierung

* [ ] Basis-Cluster bereitgestellt (AKS oder On‑prem K8s)
* [ ] Node-Pools: `system` + `workload` erstellt
* [ ] Optionaler GPU-Pool (`gpu`, taints/labels) bereitgestellt
* [ ] Container Registry angebunden (ACR/GHCR) + Pull-Rechte gesetzt
* [ ] StorageClass (Premium SSD) und Default-Storage gesetzt

### GitOps & Basis-Addons

* [ ] Argo CD installiert (App-of-Apps Schablone im Git)
* [ ] Ingress-Controller (NGINX oder Traefik) bereitgestellt
* [ ] cert-manager + ClusterIssuer (Let’s Encrypt / Interne CA) aktiv
* [ ] DNS-Einträge für Ingress-LB erstellt (A/AAAA/CNAME)

### Sicherheit (Baseline)

* [ ] OIDC/Workload Identity aktiviert (AKS: OIDC Issuer)
* [ ] NetworkPolicies Default-Deny (Clusterweiten Baseline-NS) aktiv
* [ ] Pod Security (Baseline/Restricted) durchgesetzt
* [ ] Private Registry-Only (ImagePullPolicy/Registry-Allowlist) gesetzt

### Skalierung & Resilienz

* [ ] Cluster Autoscaler aktiviert (Richtlinien dokumentiert)
* [ ] KEDA bereit (event-driven Autoscaling, Policies definiert)
* [ ] (Optional) Karpenter Provider for Azure geplant/integriert

### Betrieb & Runbooks

* [ ] Standard-Namespaces angelegt (`platform`, `infra`, `tenants/*`)
* [ ] Standard-Quotas/LimitRanges pro Namespace definiert
* [ ] Runbooks: Provisionierung, Rollbacks, Upgrades, Node-Drain, Notfallplan
* [ ] GameDay Nr. 1 durchgeführt (Ingress-Fail, Node-Fail, Scale-Test)

### DoD (Definition of Done)

* [ ] `kubectl get nodes` & Ingress/TLS grün
* [ ] Argo CD synchronisiert „Basis-Apps“ fehlerfrei
* [ ] Smoke-Tests (Ingress → Demo-Service) erfolgreich
* [ ] ADR + Runbooks versioniert und auffindbar

---

## Phase A2 – Security & Compliance

**Auftrag:** Secrets/Policies, Supply-Chain, Identity, Compliance.

### Secrets & Identity

* [ ] External Secrets Operator (ESO) installiert
* [ ] Azure Key Vault (oder HashiCorp Vault on‑prem) angebunden (SecretStore)
* [ ] Workload Identity (OIDC) für ESO-ServiceAccount konfiguriert
* [ ] Richtlinie: „Keine Secrets in Git/Pipelines“ kommuniziert und geprüft

### Policies & Härtung

* [ ] OPA Gatekeeper installiert
* [ ] Constraints aktiv:

  * [ ] Disallow `:latest`
  * [ ] Ressourcenmindestwerte (requests/limits)
  * [ ] Zugelassene Registries
  * [ ] Pod Security Level (enforced)
  * [ ] Default-Deny Netz (zus. mit NetPols)

### Supply-Chain Security

* [ ] SBOM-Erzeugung (Syft) in CI verankert
* [ ] Container-Scan (Trivy/Grype) + Schwellwerte (CVSS) als Gate
* [ ] Image-Signing (Cosign) + Verify im Admission-Pfad
* [ ] Secrets-Scanner (Gitleaks) in Commit-/PR-Gates aktiv

### Compliance (GDPR/EU-AI-Act ready)

* [ ] DPIA-Template bereit (Datenflüsse Prompt/Kontext/Logs)
* [ ] Retention-Policy pro Datenklasse definiert
* [ ] Human-in-the-Loop & Tool-Allowlists dokumentiert

### DoD

* [ ] Test-Workload bezieht Secrets aus Key Vault erfolgreich
* [ ] Verweigerungs-Tests zeigen, dass Policies greifen
* [ ] CI blockiert unsignierte/unsichere Images

---

## Phase A3 – Observability & FinOps

**Auftrag:** Logs/Metriken/Traces, Dashboards, Alerts, Kosten-Transparenz.

### Telemetrie-Pfade

* [ ] OpenTelemetry Collector installiert (Gateway/Agent-Modus)
* [ ] Prometheus→Thanos (Langzeit), Scrape-Targets definiert
* [ ] Loki + Promtail aktiv (Log-Labels standardisiert)
* [ ] Tempo/Jaeger aktiv (Traces mit Service/Version Labels)

### Dashboards & Alerts

* [ ] Grafana Dashboards: Cluster, Namespace/Tenant, Ingress, Storage, Workload
* [ ] SLO-Dashboards (P95 Latenzen, Fehlerbudgets) erstellt
* [ ] Alertmanager-Routen (P1/P2/P3) inkl. Bereitschaft

### FinOps

* [ ] Kosten-Tags & Label-Schema verbindlich
* [ ] Erste Kosten-Dashboards pro Tenant/Namespace
* [ ] Budget-Alerts (80/90/100%) definiert

### SIEM/Compliance

* [ ] Vorwärtsleitung relevanter Logs an SIEM (z. B. Defender/Sentinel/Elastic)
* [ ] Retention umgesetzt (Logs/Metriken/Traces)

### DoD

* [ ] Synthetic Smoke-Checks sichtbar (Grün/Rot)
* [ ] Alert-Test: On‑Call erhält P1-Benachrichtigung

---

## Phase B1 – Project-Factory & Team-Directory

**Auftrag:** Tenants & Projekte „per Knopf“.

### Templates & Automation

* [ ] Namespace-Template (Namespace, Quota, LimitRange, NetPol)
* [ ] Argo-App-Template (CreateNamespace=true, Repo/Pfad variabel)
* [ ] ADO-Bootstrap (Projekt, Repo, Pipeline, Boards, Teams) automatisiert
* [ ] Team-Directory YAML → CODEOWNERS/ADO-Gruppen Sync

### Prozesse

* [ ] Self-Service-Formular (Parameter: Name, Owner, Budget, SLA)
* [ ] Genehmigungsfluss (Security/FinOps Check) definiert

### DoD

* [ ] „Neuer Tenant < 30 min“ demonstriert
* [ ] Audit-Log des Provisionierungsprozesses vorhanden

---

## Phase B2 – Control-Plane (n8n)

**Auftrag:** Event-Ingress, Approvals, Benachrichtigungen.

### Architektur & Betrieb

* [ ] n8n in Queue-Topologie: Main, Webhook, Worker
* [ ] Redis bereit (Queue/Rate/State)
* [ ] SSO/RBAC für n8n-UI, Audit-Logs aktiv

### Flows (Minimal-Set)

* [ ] Service Hook (ADO) → Validierung → Dispatch (Orchestrator/Gateway)
* [ ] Approval-Flow (Manual Gate) mit Eskalation
* [ ] Benachrichtigungen (Teams/Slack/Email) standardisiert

### Resilienz

* [ ] Retry/Backoff-Strategie konfiguriert
* [ ] DLQ-Pfade dokumentiert

### DoD

* [ ] Test-Hook erzeugt Dispatch in < 1 s
* [ ] Approval klickbar, Audit-Eintrag vorhanden

---

## Phase B3 – Agent-Gateway (MCP-Hub)

**Auftrag:** Einheitlicher Agent/KI-Anschluss.

### API & Auth

* [ ] OpenAPI definiert (AuthZ/Rate/Quota/Audit)
* [ ] OIDC integriert (per Tenant Scopes)
* [ ] mTLS (intern), TLS (extern) aktiv

### Funktionen

* [ ] MCP-Brücke inkl. ADO-MCP-Server angebunden
* [ ] Quotas/Rates pro Tenant konfiguriert
* [ ] Audit-Events in Postgres gespeichert

### DoD

* [ ] Load-Test: stabil bis Ziel-RPS, korrekte Quota-Abwürfe
* [ ] Audit-Abfragen zeigen End-to-End Trace-ID

---

## Phase C1 – Orchestrator & Workflows

**Auftrag:** Sub-Workflows, Retries, Idempotenz, DLQ.

### Kern

* [ ] Temporal (oder Azure Durable) bereit
* [ ] Workflow-Gestaltungsrichtlinien (Child-Workflows, Sagas)
* [ ] Idempotency-Key-Standard + Correlation/Trace-ID
* [ ] Backoff/Retry/Jitter je Activity definiert
* [ ] DLQ + Replay-Prozedur

### Referenz-Workflows

* [ ] Ingest → Plan → Repo-Ops (Branch/Draft-PR)
* [ ] Pipeline-Factory → CI anstoßen
* [ ] Build/Test → Fix-Loop
* [ ] Reviewer/Governance → Merge/Doku

### DoD

* [ ] „Ticket→Draft‑PR“ E2E < 5 min (kleiner Diff)
* [ ] Gezielter Fail in Sub-Workflow bleibt lokal, Parent bleibt stabil

---

## Phase C2 – Adapters & Repo-Ops

**Auftrag:** ADO/GitHub/GitLab anbinden; PRs & Diffs.

### Adapter

* [ ] ADO REST/GraphQL Client (Rate‑Limit‑Robustheit)
* [ ] Git-Operationen (Branch, Merge, Diff) kapseln
* [ ] CODEOWNERS/Labels/Status-Checks API

### Repo-Ops

* [ ] Draft‑PR erstellen/aktualisieren
* [ ] Idempotente Writes (PR-Update via Idempotency-Key)
* [ ] Fehlerklassen (409, 404, 429) sauber behandeln

### DoD

* [ ] 429-Test: Exponential Backoff + Jitter greift
* [ ] Mehrfachaufruf ändert PR nicht doppelt

---

## Phase C3 – Pipeline-Factory & Runners

**Auftrag:** CI/CD standardisieren, Policy-Gates.

### Vorlagen & Gates

* [ ] YAML-Templates (Build/Test/Lint/SBOM/Scan/Sign/Verify)
* [ ] Conftest/OPA-Checks im CI
* [ ] Artifacts/Cache/ACR Push eingerichtet

### Runner-Pools

* [ ] Containerisierte Runner mit festen Toolchains
* [ ] Kapazitätsplanung + Autoscaling

### DoD

* [ ] Neues Repo → „Green by default“ Pipeline
* [ ] Unsigned/unsichere Images werden geblockt

---

## Phase D1 – Data & Retrieval

**Auftrag:** PG/Redis/MinIO/Vector-DB/Indexer/MLflow/Kosten-Ledger.

### Datenebene

* [ ] PostgreSQL-HA (PITR/WAL), Tenant-Schemata
* [ ] Redis-Cluster (Cache/Rate/Idempotenz)
* [ ] MinIO/S3 (Versioning + Object‑Lock)
* [ ] Vector-DB (pgvector/Qdrant/Weaviate) mit ACLs

### Dienste

* [ ] Indexer (Embeddings/Summaries) + Schreibpfad in Vector-DB
* [ ] MLflow Registry (Modelle/Versionen/Permissions)
* [ ] Kosten-Ledger: API + DB-Schema (Task/Tenant/Modell/Token/Runtime)

### DR/BCP

* [ ] RTO/RPO je Datenklasse dokumentiert
* [ ] Backup/Restore-Drills durchgeführt

### DoD

* [ ] RAG-Query liefert Quelle + Zugriffsprüfung
* [ ] Kosten-Ledger schreibt bei Test-Calls

---

## Phase D2 – LLM Platform & Router

**Auftrag:** Modell-Routing, Self‑Hosted Serving, Cloud-Anbindung, Guardrails.

### Router & Policies

* [ ] OpenAI‑kompatible API (Tokens/Costs/Latency Budget)
* [ ] Policy-Engine: Modellwahl nach Kosten/Privacy/SLAs
* [ ] Quotas/Budgets pro Tenant (Admission Control)

### Serving

* [ ] vLLM/TGI Deployments auf GPU-Pool (concurrency, KV‑Cache)
* [ ] Canary-Rollouts + Auto‑Rollback (SLO/Kosten)
* [ ] Cloud‑LLM Provider eingebunden (Data‑Boundary Flags)

### Safety & Evaluation

* [ ] Golden‑Sets/Regression‑Suiten
* [ ] Output‑Moderation & PII‑Redaktion

### DoD

* [ ] Self‑hosted & Cloud‑Modus A/B getestet
* [ ] Canary 10% mit Rollback bei Verletzung

---

## Phase D3 – Docs-Generator

**Auftrag:** README/CHANGELOG/ADR automatisiert.

### Funktionen

* [ ] Templates (Jinja/Handlebars) für README/ADR/CHANGELOG
* [ ] PR‑Bot mit Commit‑Signoff
* [ ] Release‑Notes aus Git-Tags/Conventional Commits

### DoD

* [ ] Merge triggert Changelog-Update‑PR
* [ ] ADR-Änderungen automatisiert erzeugt

---

## Phase E1 – Admin‑Console & Enablement

**Auftrag:** Self‑Service, Budgets/SLOs, Übersicht.

### UI & Auth

* [ ] Next.js/React App, OIDC (SSO), RBAC (Rollen/Mandant)
* [ ] OpenAPI‑Clients zu Gateway/Observability/Cost

### Funktionen

* [ ] Tenant/Projekt-Anlage (via Project‑Factory)
* [ ] Budgets & Quotas setzen, SLO‑Übersicht
* [ ] Audit‑Ansichten (Events/Traces)

### DoD

* [ ] E2E: Neuer Tenant über UI, in < 30 min nutzbar
* [ ] Budget‑Breach erzeugt sichtbare Warnung/Blockade

---

# Anhänge – gemeinsame Qualitätskriterien

* [ ] Jede App/Komponente: HealthProbes, OTel‑Instrumentierung, Ressourcen (requests/limits)
* [ ] Jede API: OpenAPI‑Spezifikation, AuthZ/Rate/Quota, strukturierte Logs
* [ ] Jedes Repo: README, ADR‑0, CI mit SBOM+Scan+Sign+Verify
* [ ] Rollback‑Strategie & Change‑Freeze vor „großen“ Upgrades dokumentiert
* [ ] GameDays/DR‑Drills im Quartal terminiert

---

# Tech-Stack‑Legende (Warum & Wofür) – A1 → E1

> Ziel: Jede Abkürzung in **einfachen Worten**: *Was ist es? Warum brauchen wir das? Wofür setzen wir es ein? Woran merken wir, dass es fertig ist?* (Plus **Stolpersteine** & **Skalierungseffekt**.)

## A1 – Platform & Delivery (Kubernetes)

### AKS / K8s (on‑prem)

* **Was?** Kubernetes als Plattform – gemanagt (**AKS**) oder selbst betrieben (on‑prem).
* **Warum?** Standardisierte, robuste Laufzeit; Self‑Healing; Rolling Updates; deklarativ.
* **Wofür?** Trägt alle Microservices, Agenten, Router, Datenservices.
* **Fertig wenn…** Nodes „Ready“, RBAC aktiv, Basis‑Namespaces vorhanden, Upgradeprozess beschrieben.
* **Stolpersteine:** Zu kleiner System‑Pool; fehlende Reserven; vernachlässigte Upgrades.
* **Skalierungseffekt:** Mehr/gezielte Node‑Pools → mehr parallele Nutzer/Jobs.

### Argo CD (GitOps)

* **Was?** Controller, der Cluster‑Ist mit Git‑Soll abgleicht.
* **Warum?** Nachvollziehbare Deploys, schnelle Rollbacks, Single Source of Truth.
* **Wofür?** Alle Add‑ons & später App‑Deployments („App‑of‑Apps“).
* **Fertig wenn…** Drift=0, Rollback getestet, Rechte/Rollen gesetzt.
* **Stolpersteine:** Direktänderungen im Cluster (an Git vorbei) → Chaos.
* **Skalierungseffekt:** Viele Teams deployen parallel – Argo hält Ordnung.

### Ingress (NGINX/Traefik)

* **Was?** HTTP‑Eingangstor für Services.
* **Warum?** Einheitliche URL, TLS, Routing, Rate‑Limits, Blue/Green.
* **Wofür?** Zugang zu UIs/APIs/Router.
* **Fertig wenn…** Test‑Service per HTTPS erreichbar, Regeln versioniert.
* **Stolpersteine:** Fehlende Timeouts/Rate‑Limits.
* **Skalierungseffekt:** Horizontal skalierbar; mit **Front Door + WAF** global absicherbar.

### cert‑manager + ClusterIssuer

* **Was?** Automatisches Ausstellen/Erneuern von TLS‑Zertifikaten.
* **Warum?** Kein Zertifikats‑Kuddelmuddel; weniger Ausfälle.
* **Wofür?** TLS für alle Ingress‑Hosts.
* **Fertig wenn…** Zertifikate automatisch erstellt/erneuert.
* **Stolpersteine:** DNS/ACME falsch konfiguriert.
* **Skalierungseffekt:** Viele Subdomains ohne Mehraufwand.

### Container Registry (ACR/GHCR)

* **Was?** Sicherer Speicher für Container‑Images.
* **Warum?** Schnelles, policy‑konformes Pulling; Signaturen & Retention.
* **Wofür?** Alle Build‑Images; Deploy zieht von hier.
* **Fertig wenn…** Pull‑Rechte (AcrPull) gesetzt, Signatur‑Verify aktiv.
* **Stolpersteine:** Unsignierte/öffentliche Images; fehlende Aufräumregeln.
* **Skalierungseffekt:** Geo‑Replikation beschleunigt global.

### StorageClass (Premium SSD)

* **Was?** Vorgabe für persistente Volumes.
* **Warum?** Planbare Performance & Verfügbarkeit; konsistente Defaults.
* **Wofür?** DBs, Logs, Caches, Operatoren.
* **Fertig wenn…** Default‑StorageClass gesetzt; IOPS/Latenz ok.
* **Stolpersteine:** Falsche Klasse → Latenzspikes.
* **Skalierungseffekt:** Viele Volumes mit stabiler Leistung.

### DNS / Azure Front Door / WAF

* **Was?** Namensauflösung + globaler Edge‑Proxy + Web‑Firewall.
* **Warum?** Sichere, performante externe Erreichbarkeit; DDoS/Geo/Ratelimit.
* **Wofür?** Öffentlicher Zugang zu UIs/APIs.
* **Fertig wenn…** Domain → Ingress/Front Door; WAF‑Regeln aktiv.
* **Stolpersteine:** Fehlende Redirects/TLS‑Hygiene.
* **Skalierungseffekt:** Anycast/Edge‑Caching für viele Nutzer.

### Cluster Autoscaler

* **Was?** Skaliert **Nodes** automatisch je Bedarf.
* **Warum?** SLA & Kosten im Gleichgewicht; kein Handbetrieb.
* **Wofür?** Absorbiert Workload‑Peaks.
* **Fertig wenn…** Scale‑Out/In unter Last belegt.
* **Stolpersteine:** Unpassende Pod‑Limits blockieren Scheduling.
* **Skalierungseffekt:** Automatischer Kapazitätszuwachs.

### Karpenter (Azure Provider)

* **Was?** Schneller Node‑Provisioner.
* **Warum?** Kürzere Startzeiten; passgenaue Instanztypen.
* **Wofür?** Peaks, GPU/High‑Mem Workloads.
* **Fertig wenn…** Provisionierung < Minuten, richtiger Typ gewählt.
* **Stolpersteine:** Zu breite Policies → teuer.
* **Skalierungseffekt:** Kürzere Warteschlangen.

### KEDA (Event‑Driven Autoscaling)

* **Was?** Skaliert **Pods** nach Queue/RPS/PromQL.
* **Warum?** Reagiert schneller als CPU‑HPA; ideal bei spiky Last.
* **Wofür?** Workers, Inferenz, Batch.
* **Fertig wenn…** Scale‑to‑Zero & Queue‑Skalierung getestet.
* **Stolpersteine:** Flapping ohne Cool‑Down.
* **Skalierungseffekt:** Automatisch mehr Pods bei mehr Nutzern.

### OIDC / Workload Identity

* **Was?** Kurzlebige Pod‑Identitäten statt statischer Secrets.
* **Warum?** Weniger Leck‑Risiko; Least‑Privilege auf Cloud‑APIs.
* **Wofür?** Zugang zu Key Vault, Storage, Registry.
* **Fertig wenn…** ESO/Apps sprechen ohne statische Keys.
* **Stolpersteine:** Falsche Federated Credentials.
* **Skalierungseffekt:** Kein Credential‑Wildwuchs.

### NetworkPolicies

* **Was?** Ost‑West‑Firewall auf Pod‑Ebene.
* **Warum?** Mandanten‑/Service‑Isolation.
* **Wofür?** Default‑Deny + explizite Erlaubnisse.
* **Fertig wenn…** Unerlaubte Verbindungen scheitern.
* **Stolpersteine:** DNS/Egress vergessen.
* **Skalierungseffekt:** Sicherheit bleibt beherrschbar.

### Pod Security Standards

* **Was?** Sicherheitslevel (Baseline/Restricted).
* **Warum?** Minimiert Container‑Angriffsfläche.
* **Wofür?** Clusterweite Baseline.
* **Fertig wenn…** Verstöße werden abgelehnt.
* **Stolpersteine:** Alte Images benötigen verbotene Rechte.

### ImagePullSecrets via ESO

* **Was?** Registry‑Zugänge aus Secret‑Store.
* **Warum?** Keine Klartexte in Git/CI.
* **Wofür?** ACR/GHCR Zugriff.
* **Fertig wenn…** Pull funktioniert ohne Handarbeit.
* **Stolpersteine:** Falscher Namespace/Scope.

### RBAC

* **Was?** Rollen & Rechte.
* **Warum?** Least‑Privilege, klare Zuständigkeiten.
* **Wofür?** Team‑Namespaces, Admin‑Tasks.
* **Fertig wenn…** Rollen zugewiesen, Audit ok.
* **Stolpersteine:** Over‑privileged ServiceAccounts.

---

## A2 – Security & Compliance

### OPA/Gatekeeper

* **Was?** Policy‑Engine (Manifeste gegen Regeln).
* **Warum?** Einheitliche Sicherheitsregeln erzwingen.
* **Wofür?** Kein `:latest`, Ressourcengrenzen, Registry‑Allowlist, Pod Security.
* **Fertig wenn…** Verstöße werden geblockt.
* **Stolpersteine:** Zu strenge Regeln blocken legitime Deploys.

### ESO – External Secrets Operator

* **Was?** Holt Secrets aus Key Vault/Vault ins Cluster.
* **Warum?** Zentrale Verwaltung, Rotation, Audit.
* **Wofür?** DB‑Passwörter, API‑Keys, Registry‑Creds.
* **Fertig wenn…** Workloads beziehen Secrets dynamisch.
* **Stolpersteine:** Rechte-/Federation‑Fehler.

### Azure Key Vault / HashiCorp Vault

* **Was?** Secret/Key/Cert‑Tresor.
* **Warum?** Sichere Ablage, Rotation, RBAC, Audit.
* **Wofür?** Alle sensitiven Werte.
* **Fertig wenn…** Zugriff nur via OIDC/ESO; Rotationsplan steht.
* **Stolpersteine:** Manuelle Schleichwege („Key kopiert“).

### Gitleaks

* **Was?** Secret‑Scanner für Repos/PRs.
* **Warum?** Leaks früh stoppen.
* **Wofür?** Pre‑commit/CI/PR‑Gate.
* **Fertig wenn…** Leaks blockiert, Revoke/Rotate‑Runbook existiert.

### Cosign

* **Was?** Image‑Signatur/Verifikation.
* **Warum?** Supply‑Chain‑Integrität.
* **Wofür?** Nur signierte Images deployen.
* **Fertig wenn…** Unsigned wird abgelehnt.

### Trivy / Grype & Syft (SBOM)

* **Was?** Vuln‑Scanner & Stückliste.
* **Warum?** CVEs sichtbar; Gates je Schwelle.
* **Wofür?** CI‑Gates; SBOM als Artefakt.
* **Fertig wenn…** Kritische CVEs blocken Build; SBOM archiviert.

### Entra ID / Keycloak (SSO)

* **Was?** Identitäts‑Provider.
* **Warum?** Einheitliche Anmeldung, MFA, Rollen.
* **Wofür?** Argo/n8n/UI/Grafana/Gateway.
* **Fertig wenn…** Logins & Rollen geprüft.

### DPIA & EU‑AI‑Act Checks

* **Was?** Datenschutz‑/KI‑Regel‑Prüfung.
* **Warum?** Rechtssicherheit & Audit‑Readiness.
* **Wofür?** Datenflüsse, Retention, Human‑in‑the‑Loop.
* **Fertig wenn…** Vorlagen ausgefüllt, freigegeben.

---

## A3 – Observability & FinOps

### OpenTelemetry Collector

* **Was?** Einheitlicher Ingest für Logs/Metriken/Traces.
* **Warum?** Standardisiert & vendor‑neutral.
* **Wofür?** Export zu Prom/Thanos, Loki, Tempo & SIEM.
* **Fertig wenn…** Correlierbare Signale sichtbar.

### Prometheus → Thanos

* **Was?** Metriken & Langzeit‑/Multi‑Cluster‑Abfragen.
* **Warum?** SLO‑Tracking, Kapazitätsplanung.
* **Wofür?** Cluster/Workload/Business‑Metriken.
* **Fertig wenn…** SLO‑Dashboards & Alerts aktiv.

### Grafana

* **Was?** Visualisierung & Alert‑UI.
* **Warum?** Gemeinsame Lagebilder.
* **Wofür?** Cluster/Tenant/Service‑Sichten.
* **Fertig wenn…** On‑Call nutzt sie täglich.

### Loki / Promtail & Tempo/Jaeger

* **Was?** Logs & Traces.
* **Warum?** Schnelles Debugging, Root Cause.
* **Wofür?** Trace‑ID durch Gateway→Orchestrator→Service.
* **Fertig wenn…** Suche nach Trace/Labels klappt.

### Alertmanager

* **Was?** Alarm‑Router (P1–P3, Eskalationen).
* **Warum?** Nichts geht unter.
* **Fertig wenn…** Testalarm erreicht Bereitschaft.

### Azure Monitor / Log Analytics (optional)

* **Was?** Cloud‑Monitoring & SIEM.
* **Warum?** Unternehmensweite Korrelation.

### Cost‑Tags & Kosten‑Dashboards

* **Was?** Labels (`env/tenant/app/cost‑center`) + Grafana‑Sichten.
* **Warum?** Sichtbare Kosten je Team/Projekt.
* **Fertig wenn…** Budget‑Alerts feuern (80/90/100%).

---

## B1 – Project‑Factory & Team‑Directory

### Argo App‑Templates

* **Was?** Schablonen für App‑/Namespace‑Anlage.
* **Warum?** Schnell, konsistent, compliant.
* **Wofür?** „Neuer Tenant < 30 min“.
* **Fertig wenn…** Formular → Merge → Namespace+App live.

### YAML‑Schablonen (NS/Quota/LimitRange/NetPol)

* **Was?** Standard‑Bausteine je Team‑Namespace.
* **Warum?** Ressourcenbegrenzung & Isolation automatisch.

### ADO‑Bootstrap & CODEOWNERS

* **Was?** Projekt/Repo/Pipeline/Boards automatisch; Reviewer‑Pflichten.
* **Warum?** Start ohne Wartezeit; klare Verantwortungen.
* **Fertig wenn…** Neues Projekt sofort commit/deploy‑fähig.

---

## B2 – Control‑Plane (n8n)

### n8n (Queue‑Mode) + Redis

* **Was?** Low‑Code‑Automationen + Queue/Rate/State.
* **Warum?** Schnelle Integrationen; robuste Verarbeitung.
* **Wofür?** ADO‑Hooks → Validierung → Dispatch/Approval → Notif.
* **Fertig wenn…** Flows stabil, Retry/Backoff & DLQ dokumentiert.

### Service Hooks (ADO) & SSO/RBAC

* **Was?** Events (PR/Tickets/Builds) + gesicherter Zugriff.
* **Warum?** Event‑getriebene Plattform, kein Polling.

---

## B3 – Agent‑Gateway (MCP‑Hub)

### API‑Gateway (AuthZ/Rate/Quota/Audit)

* **Was?** Einheitliches Tor mit Sicherheit/Limits.
* **Warum?** Schutz vor Missbrauch; faire Verteilung.
* **Wofür?** Alle Agenten/Tools gehen hier durch.
* **Fertig wenn…** OpenAPI, OIDC, mTLS, Audit‑DB aktiv.

### MCP + ADO‑MCP Server

* **Was?** Standard zum Andocken von Agenten & Tools.
* **Warum?** Einheitliche Tool‑Schnittstelle; weniger Kopplung.
* **Fertig wenn…** Externe/Interne Agenten funktionieren über MCP.

### PostgreSQL (Audit‑DB)

* **Was?** Revisionssichere Audit‑Logs/Quotas.
* **Warum?** Nachweis, Abrechnung, Forensik.

---

## C1 – Orchestrator & Workflows

### Temporal / Azure Durable

* **Was?** Zustandsbehaftete Workflows (Retries/Timers/Child‑Flows).
* **Warum?** Fehlertoleranz; Wiederaufnahme ohne Doppel‑Effekte.
* **Wofür?** Ticket→Draft‑PR→Build/Test→Review→Merge.
* **Fertig wenn…** Sub‑Workflow‑Fail bleibt lokal; E2E < 5 min (klein).

### RabbitMQ / Kafka / Redis Streams

* **Was?** Queues/Streams für Jobs/Events.
* **Warum?** Entkopplung, Backpressure, horizontale Skalierung.

### Idempotency Keys / DLQ / Sagas

* **Was?** Doppelvermeidung; Fehler‑Ablage; Rollback‑Schritte.
* **Warum?** Korrekte Wiederholungen; saubere Korrekturen.

---

## C2 – Adapters & Repo‑Ops

### ADO REST/GraphQL & Git‑Ops

* **Was?** Programmgesteuerte Board/Repo/Pipeline‑Steuerung + Git‑Operationen.
* **Warum?** Automatisierung; kleine, sichere Diffs.
* **Wofür?** Draft‑PRs, Labels, CODEOWNERS, Status‑Checks.
* **Fertig wenn…** 429‑Backoff, Idempotenz, Konflikt‑Handling belegt.

---

## C3 – Pipeline‑Factory & Runners

### YAML‑Templates & Conftest/OPA

* **Was?** Standard‑Pipelines + Policy‑Gates.
* **Warum?** „Green by default“; Compliance by design.

### ADO Pipelines & Runner‑Pools

* **Was?** Ausführung von Builds/Tests/Scans.
* **Warum?** Skalierbar, reproduzierbar, schnell.
* **Fertig wenn…** Neues Repo baut grün; Unsigned/unsichere Images blockiert.

### Artifacts/Cache & ACR

* **Was?** Schnellere Builds; reproduzierbare Artefakte; gesicherte Images.

---

## D1 – Data & Retrieval

### PostgreSQL‑HA

* **Was?** Transaktions‑/Audit‑DB (HA, PITR/WAL).
* **Warum?** Konsistente, nachvollziehbare Plattformdaten.
* **Wofür?** Audit, Ledger, Metadaten, Tenants (Schema‑Trennung).
* **Fertig wenn…** Failover/Restore‑Drill bestanden.

### Redis Cluster

* **Was?** In‑Memory Cache/Rate/Idempotenz.
* **Warum?** Niedrige Latenz; Schutz vor Thundering Herd.

### MinIO / S3 (Versioning, Object‑Lock)

* **Was?** Objekt‑Storage für Artefakte/Logs/Modelle.
* **Warum?** Nachverfolgbarkeit; WORM bei Bedarf.

### Vector‑DB (pgvector/Qdrant/Weaviate) & Indexer

* **Was?** Semantische Suche (Embeddings) + Write‑Pfad.
* **Warum?** RAG‑Kontext, bessere Antworten.

### MLflow

* **Was?** Modell‑Registry (Version/Stages/Lineage).
* **Warum?** Kontrollierte Modell‑Lebenszyklen.

### Kosten‑Ledger

* **Was?** Task/Tenant‑bezogene Kosten (Tokens/Runtime/Modell).
* **Warum?** FinOps/Quotas/Abrechnung.

### RTO/RPO & Drills

* **Was?** Wiederanlauf‑/Datenverlust‑Ziele + Übungen.
* **Warum?** Resilienz & Audit.

---

## D2 – LLM Platform & Router

### Model‑Router (OpenAI‑Compat)

* **Was?** Ein API‑Endpunkt für alle Modelle.
* **Warum?** Kosten/Latenz/Datenschutz steuerbar; Caching/Quotas.
* **Wofür?** Auswahl Self‑hosted vs. Cloud; Canary/AB.

### vLLM / TGI (GPU‑Serving)

* **Was?** High‑Throughput Inferenz (KV‑Cache, Tensor‑Optimierungen).
* **Warum?** Gute Latenz/Kosten bei viel Traffic.
* **Fertig wenn…** Concurrency+Cache getuned; SLOs gehalten.

### Cloud‑LLMs (Azure/OpenAI/Anthropic/Google)

* **Was?** Managed Inferenz mit Data‑Boundary/No‑Log.
* **Warum?** Spitzen abfangen; Spezialmodelle.

### Guardrails (PII/Moderation/Tool‑Allowlist)

* **Was?** Sicherheits‑/Compliance‑Schutz.
* **Warum?** Policy‑konforme Antworten.

### KEDA & Canary

* **Was?** Autoscaling nach Last; progressive Auslieferung + Auto‑Rollback.

---

## D3 – Docs‑Generator

### FastAPI/Node + Templates (Jinja/Handlebars)

* **Was?** Service erzeugt README/ADR/CHANGELOG.
* **Warum?** Doku aktuell ohne Handarbeit.
* **Fertig wenn…** Merge → Doku‑PR automatisch.

### Conventional Commits & Signoff

* **Was?** Strukturierte Commits; Nachvollziehbarkeit.

---

## E1 – Admin‑Console & Enablement

### Next.js/React + OpenAPI‑Clients

* **Was?** UI für Projekte/Tenants/Budgets/SLOs/Audit.
* **Warum?** Self‑Service; Transparenz für viele Teams.
* **Fertig wenn…** Neuer Tenant per UI in < 30 min nutzbar.

### RBAC/SSO & Scorecards

* **Was?** Rollenbasierte Sicht/Steuerung; KPI‑Übersicht.
* **Warum?** Steuerbarkeit bei vielen Nutzern/Teams.
