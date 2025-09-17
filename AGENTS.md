# Prompt für GPT-5 Codex (final)

**Kontext & Quelle**
Projekt/Repo: [https://github.com/ADASK-B/Coding-Assistent](https://github.com/ADASK-B/Coding-Assistent)
Site: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/)
Markdown-Viewer: [https://adask-b.github.io/Coding-Assistent/md.html?path=goal/goal.md](https://adask-b.github.io/Coding-Assistent/md.html?path=goal/goal.md)

**Nicht verhandelbare Regeln**

1. **Alle** neuen Dokumente (Markdown) **nur** unter `docs/` anlegen.
2. Unterordner sind erlaubt (analog `docs/goal/goal.md`).
3. Interne Links **immer** über das Viewer-Schema setzen:
   `md.html?path=<relativer-Pfad-ab-docs>` (Beispiel: `md.html?path=monitoring/monitoring.md`).
4. **Kein** Inhalt außerhalb von `docs/` (keine Doppelablagen).
5. Einheitliches Format wie `goal/goal.md`: Titel, Zweck, Enterprise-Anforderungen, Technologie-Stack, Deliverables, Offene Punkte/Abhängigkeiten.
6. Inhalte **real** ausarbeiten (keine Lorem-Ipsum-Platzhalter). Grundlage sind `goal.md`, die sichtbaren Bereiche der Startseite und die vorgegebenen Technologien **Redis** und **Neon**. Startseiten-Referenz: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])

---

## Aufgaben

### A) Verzeichnisstruktur unter `docs/` anlegen

Erstelle folgende Ordner und Dateien (alle Pfade relativ zu `docs/`):

* `goal/goal.md` (bestehend, ggf. verlinken; Quelle bleibt)
* `core/core.md`
* `monitoring/monitoring.md`

  * `monitoring/prometheus.md`
  * `monitoring/grafana.md`
  * `monitoring/loki.md`
  * `monitoring/alertmanager.md`
  * `monitoring/cadvisor.md`
  * `monitoring/node-exporter.md`
  * `monitoring/promtail.md`
  * `monitoring/health-monitor.md`
* `security/security.md`
* `tenancy/tenancy.md`

  * `tenancy/operations.md`
* `data/data.md`  *(Data Layer mit **Redis** & **Neon Postgres**)*

  * `data/redis.md`
  * `data/neon.md`
* `deployment/deployment.md` *(One-Command / Docker Compose; Übergang zu K8s verlinken)*
* `setup/setup.md` *(Quick Setup Guide)*
* `requirements/requirements.md` *(System Requirements)*
* `examples/examples.md` *(Usage Examples)*
* `adr/README.md` + mindestens zwei ADRs:

  * `adr/ADR-0001-llm-provider-agnostic.md`
  * `adr/ADR-0002-rag-vs-context-only.md`
* `architecture/c4.md` *(Verweis auf C4/Structurizr-DSL, Generierungshinweise)*
* `compliance/compliance.md`
* `governance/governance.md` *(Security & Governance, Audit, Secrets, RBAC/OIDC)*
* `k8s/k8s.md` *(Cloud-Native Deployment / K8s Migration, Helm, GitOps, HPA/KEDA)*
* `operations/operations.md` *(Runbooks, Health-Checks, Troubleshooting)*
* `quality/quality.md` *(Quality Framework & Testing, Golden Tests, Gates)*
* `evaluation/evaluation.md` *(Evaluation Framework, Benchmarks)*
* `api/api.md` *(exponierte APIs, Webhooks, Rate-Limits, Auth)*
* `integration/azure-devops.md` *(PR-Automation, Webhooks, Status Updates)*
* `dr/dr.md` *(Backup & Disaster Recovery)*
* `slo/slo.md` *(SLO/SLI/SLA, Error Budgets)*
* `risk/risk.md` *(Risk Register)*
* `glossary.md`
* `roadmap.md`

> Begründung der Top-Level-Sektionen: Entspricht sichtbar vorhandenen Bereichen der Startseite (Core Services, Monitoring, Security Layer, Multi-Tenant Isolation, K8s Migration, Setup, Requirements, Examples, ADRs, C4, Compliance, Operations) und ergänzt Enterprise-Pflichtbereiche (Data Layer, Governance, DR, SLO, Risk, Glossar, Roadmap). Quelle Startseite: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])

### B) Inhalt pro Datei (Mindeststruktur)

Für **jede** `.md`:

1. **Titel** (H1)
2. **Zweck & Scope** (Was deckt der Bereich ab? Abgrenzung)
3. **Enterprise-Anforderungen** (Security, Compliance, Skalierung, Mandantenfähigkeit, Observability, Automatisierung)
4. **Technologie-Stack** (präzise Nennung, inkl. Version/Deployment-Modus falls festgelegt)
5. **Architektur & Betrieb** (Diagramm-Verweise, Datenflüsse, Latenz/Zuverlässigkeit, HA/DR)
6. **Schnittstellen & Verträge** (APIs, Events, Schemas; AuthZ/AuthN, Raten, Limits)
7. **Deliverables** (Artefakte/Definition of Done je Bereich)
8. **Offene Punkte & Abhängigkeiten**
9. **Links** im Schema `md.html?path=...` zu Unterseiten

**Spezifika einarbeiten:**

* `monitoring/monitoring.md`: „Monitoring Stack (8)“ vollständig dokumentieren. Enthalten sein müssen **Grafana, Prometheus, Loki, Alertmanager, cAdvisor, Node Exporter, Promtail, Health Monitor**. Startseiten-Referenz vorhanden („Monitoring Stack (8)“, Live-Verweise). [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])
* `core/core.md`: Core Services von der Startseite aufnehmen (**Gateway, Orchestrator, Adapter, LLM-Patch, Traefik, ngrok, Ollama, Azurite**) mit Rollen/Interaktionen. [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])
* `data/data.md`: **Redis** (Caching, Queues, Session/Rate-Limit) und **Neon Postgres** (transaktional, langlebige Daten) als **gesetzte** Technologien. Architektur (lokal → Kubernetes → Cloud), Betriebsmodelle, Backups, Migrationsstrategie.
* `k8s/k8s.md`: Migration von Docker Compose zu Kubernetes (Helm, GitOps, Ingress, HPA/KEDA, Secrets, StorageClass), Multi-Tenant-Isolationsbezug.
* `security/security.md` + `governance/governance.md`: OIDC/OAuth2, **RBAC**, Audit Trails, Secret-Management, **Zero-Trust**.
* `tenancy/tenancy.md`: harte Tenant-Isolation (Daten, Netzwerk, Ressourcenquoten), Admin-Prozesse (`tenancy/operations.md`).
* `adr/*`: Format nach MADR oder herkömmlichem ADR-Schema, inkl. Status/Entscheidung/Konsequenzen.
* `architecture/c4.md`: Hinweise/Beispiele zur Generierung (Structurizr DSL), und Verlinkung der Diagrammausgaben.

### C) Navigation in `docs/index.html` ergänzen

1. Für **jeden** obigen Bereich einen sichtbaren Navigationspunkt anlegen (gleiche Kachel-Optik wie vorhanden).

2. **Alle** Links verwenden Schema:

   * Core Services → `md.html?path=core/core.md`
   * Monitoring Stack → `md.html?path=monitoring/monitoring.md`
   * Security Layer → `md.html?path=security/security.md`
   * Multi-Tenant Isolation → `md.html?path=tenancy/tenancy.md`
   * K8s Migration → `md.html?path=k8s/k8s.md`
   * Data Layer → `md.html?path=data/data.md`
   * Setup → `md.html?path=setup/setup.md`
   * Requirements → `md.html?path=requirements/requirements.md`
   * Examples → `md.html?path=examples/examples.md`
   * ADRs → `md.html?path=adr/README.md`
   * C4 Diagrams → `md.html?path=architecture/c4.md`
   * Compliance → `md.html?path=compliance/compliance.md`
   * Governance → `md.html?path=governance/governance.md`
   * Operations → `md.html?path=operations/operations.md`
   * Quality Framework → `md.html?path=quality/quality.md`
   * Evaluation Framework → `md.html?path=evaluation/evaluation.md`
   * API → `md.html?path=api/api.md`
   * Azure DevOps Integration → `md.html?path=integration/azure-devops.md`
   * DR/Backup → `md.html?path=dr/dr.md`
   * SLO/SLI/SLA → `md.html?path=slo/slo.md`
   * Risk Register → `md.html?path=risk/risk.md`
   * Glossary → `md.html?path=glossary.md`
   * Roadmap → `md.html?path=roadmap.md`

3. Auf der Startseite beschriebene existierende Bereiche **nicht entfernen**, sondern korrekt **verlinken** (z. B. Monitoring, Core, Security, Tenancy, K8s). Referenz: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])

### D) Qualitätskriterien (Akzeptanz)

* **Ort**: Jede neue `.md` liegt unter `docs/…` (geprüft).
* **Links**: Jeder Navigationspunkt öffnet eine `.md` via `md.html?path=…`.
* **Vollständigkeit**: Alle oben aufgelisteten Dateien existieren und sind mit **echtem** Inhalt gefüllt (kein Placeholder-Fluff).
* **Konsistenz**: Einheitlicher Stil wie `goal.md` (Abschnitte, Überschriften, Sprache).
* **Enterprise-Tauglichkeit**: Security, Compliance, Mandantenfähigkeit, Observability, DR und Governance sind **überall** adressiert.
* **Datenlage**: **Redis** und **Neon** sind im Data-Layer eindeutig dokumentiert (Architektur, Betrieb, Backup, Migration).
* **Nachvollziehbarkeit**: ADRs vorhanden und verlinkt; C4-Hinweise enthalten.
* **Index gepflegt**: `docs/index.html` enthält funktionierende Einträge für alle Bereiche.

### E) „Definition of Done“ (Lieferobjekte)

* Neu erstellte Ordner/Dateien gemäß Liste unter A).
* Aktualisierte `docs/index.html` mit korrekten `md.html?path=…`-Links.
* Interne Querverlinkungen zwischen Bereichen (z. B. Monitoring ↔ Operations, Security ↔ Governance).
* Ein Commit mit eindeutiger Nachricht, z. B.:
  `docs: add enterprise docs structure (core, monitoring, security, tenancy, data, k8s, ops, compliance, governance, quality, evaluation, api, integration, dr, slo, risk, glossary, roadmap) and update index nav`

---

## Do’s

* `docs/` als **einzige** Quelle der Wahrheit für Inhalte.
* Startseiten-Bereiche **1:1** übernehmen und mit Zieldateien verlinken. Quelle: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])
* Monitoring-Stack als **acht** Bausteine ausarbeiten (Grafana, Prometheus, Loki, Alertmanager, cAdvisor, Node Exporter, Promtail, Health Monitor).
* Data-Layer verbindlich mit **Redis** und **Neon** dokumentieren (Use-Cases, Betriebsmodelle, K8s-Pfad).
* ADR-Format konsistent halten; C4-Seite mit Generierungshinweisen.

## Don’ts

* Keine `.md` außerhalb von `docs/`.
* Keine Links ohne `md.html?path=…`.
* Keine Platzhaltertexte.
* Keine Abweichungen von der Abschnittsstruktur wie in `goal.md`.

---

**Referenzen, die du bei der Ausarbeitung berücksichtigen musst:**

* Startseite/Abschnittsnamen und laufende Inhalte: [https://adask-b.github.io/Coding-Assistent/](https://adask-b.github.io/Coding-Assistent/) ([adask-b.github.io][1])
* Markdown-Viewer-Schema (Beispiel `goal/goal.md`): [https://adask-b.github.io/Coding-Assistent/md.html?path=goal/goal.md](https://adask-b.github.io/Coding-Assistent/md.html?path=goal/goal.md) ([adask-b.github.io][2])