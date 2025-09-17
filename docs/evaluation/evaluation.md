# Evaluation Framework

## Zweck & Scope
Bewertet Leistungsfaehigkeit und Sicherheit der Agenten kontinuierlich. Definiert Benchmarks, Vergleichstests und Reporting fuer Stakeholder.

## Enterprise-Anforderungen
- Wiederholbare Evaluationspipelines, versionierte Testsets, reproducible Ergebnisse.
- Provider-Agnostische Vergleiche (LLM Router) mit Kosten- und Qualitaetsmetriken.
- Dokumentierte Bias-, Safety- und Halluzinations-Checks.
- Integration in Compliance (EU-AI-Act) und Governance Reports.
- Automatisierte Benachrichtigung bei KPI-Verletzungen.

## Technologie-Stack
- Benchmark Suites: HumanEval, CodeContests, interne Golden Tasks.
- Tooling: LangSmith, Promptfoo, OpenAI Evals, Custom Scripts.
- Data Storage: Neon (Results), Redis (Queues), Objectstore (Artifacts).
- Pipelines: Azure DevOps, Github Actions, Airflow/n8n.
- Visualization: Grafana/PowerBI Dashboards.

## Architektur & Betrieb
Evaluation Runner ziehen Model-Versionen und Prompt Configs aus Git. Jobs laufen containerisiert (K8s) mit GPU Pools. Ergebnisse werden in Neon gespeichert, Alerts generiert bei Metrikverschlechterung. Reports gehen an Quality Board, Security (Safety) und Governance.

## Schnittstellen & Vertraege
- API `/api/v1/evaluations` fuer Trigger, `/api/v1/results` fuer Abfragen.
- Event Streams fuer Roadmap (Feature Readiness), Risk Register.
- Contracts fuer Testcases (YAML/JSON Schema) und Prompt Templates.
- Webhooks zu Quality, Operations, Governance.

## Deliverables
- Benchmark Library mit dokumentierten Szenarien und Ownern.
- Evaluation Pipeline YAMLs inkl. Data Sanitization.
- Monatsberichte (Provider Performance, Drift, Safety Issues).
- Incident Playbook fuer Evaluation Fails.

## Offene Punkte & Abhaengigkeiten
- Beschaffung weiterer GPU-Ressourcen fuer umfangreiche Tests.
- Abstimmung mit Data Team fuer Sample/Masking Policies.
- Integration mit Roadmap fuer Feature Gates.
- Abhaengigkeit zu Security fuer Safety Review.

## Links
- [Quality Framework](md.html?path=quality/quality.md)
- [Risk Register](md.html?path=risk/risk.md)
- [ADR 0001](md.html?path=adr/ADR-0001-llm-provider-agnostic.md)
- [Roadmap](md.html?path=roadmap.md)
