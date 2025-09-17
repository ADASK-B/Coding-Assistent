# Quality Framework

## Zweck & Scope
Das Quality Framework definiert Metriken, Tests und Gates zur Sicherstellung hochwertiger Agentenartefakte. Es deckt Code, Dokumentation, Pipelines und AI-Ausgaben ab.

## Enterprise-Anforderungen
- Golden Test Suites fuer Kernfunktionen, Regressionen und Sicherheitschecks.
- Automatische Quality Gates in CI/CD (Lint, Tests, Security, Coverage).
- Messbare KPIs (Defect Density, MTTR, Code Review Turnaround).
- Dokumentierte Prozesse fuer Ausnahmegenehmigungen und Waivers.
- Integration mit Evaluation, Operations, Compliance und PMO.

## Technologie-Stack
- Testing: Jest/Pytest, Playwright, k6, Custom Golden Tests.
- Static Analysis: ESLint, Pylint, SonarQube, Semgrep.
- Security: Trivy, Gitleaks, Dependency-Track, Zaproxy.
- CI/CD: Azure DevOps, GitHub Actions, Make/Taskfile.
- Observability: Prometheus/Grafana Dashboards fuer Quality KPIs.

## Architektur & Betrieb
Quality Gates laufen in Pipelines (build -> test -> quality -> deploy). Golden Tests werden aus Evaluation Framework gespeist. Quality Portal (Grafana) visualisiert KPIs. Manual Overrides erfordern Governance Approval. Feedback Loops reichen bis Roadmap.

## Schnittstellen & Vertraege
- PR Template Felder fuer Tests, Evidence, Risk.
- API Hook zu Evaluation fuer Benchmark Ergebnisse.
- Integration mit Operations fuer Incident Reviews.
- Reporting an Governance/Compliance (Quality Reports).

## Deliverables
- Test-Katalog (unit, integration, e2e, golden, chaos).
- Pipeline Templates (quality.yml) mit Quality Gates.
- Reviewer Guides, Pairing Regeln, AI Output Review Checklist.
- Quality Dashboard und monatliche Reports.

## Offene Punkte & Abhaengigkeiten
- Ausbau Golden Test Library fuer LLM spezifische Szenarien.
- Integration von Code Coverage >85% in Roadmap Milestones.
- Abstimmung mit Security fuer kombinierte Gates.
- Abhaengigkeit zu Evaluation fuer Benchmarks.

## Links
- [Evaluation Framework](md.html?path=evaluation/evaluation.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Risk Register](md.html?path=risk/risk.md)
