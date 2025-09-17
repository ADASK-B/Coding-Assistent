# Architecture Decision Records

## Zweck & Scope
ADR-Ordner sammelt Architekturentscheidungen mit nachvollziehbaren Kontexten. Alle kritischen Weichenstellungen fuer Agentenplattform werden hier dokumentiert.

## Enterprise-Anforderungen
- Vollstaendige Historie mit Versionskontrolle, Reviewer und Status.
- Einhaltung von Governance-Templates (Context, Decision, Consequences).
- Verlinkung zu Compliance, Risk und Roadmap fuer Audits.
- Automatisierte Checks in CI (Linting, Broken Links, Metadata).
- Ver?ffentlichung in Docs Portal via `md.html?path=adr/<file>.md`.

## Technologie-Stack
- Markdown basierte ADRs nach Michael Nygard Format.
- ADR CLI/Plop Templates, Git Hooks fuer Naming Enforcements.
- CI Tasks (Node) fuer Markdown Lint, Link Checker.
- Integration in Structurizr/Model Repo fuer Traceability.

## Architektur & Betrieb
Neue ADRs werden ueber `npm run adr:new` erstellt, Reviewer (Architecture Board) muessen zustimmen. Statusfluesse: Proposed -> Accepted -> Implemented -> Superseded. Links zu Roadmap und Risk Register werden gepflegt. Automatisierte Publish Jobs aktualisieren Docs.

## Schnittstellen & Vertraege
- Pull Request Template fuer ADRs (Context, Decision, Rationale, Status).
- Links zu Governance, Compliance, Quality fuer Abnahme.
- Traceability zu Tickets/Features via ADO Work Item IDs.
- Change Management Approval (CAB) fuer High-Risk Entscheidungen.

## Deliverables
- ADR Index mit Tags (Security, Data, Observability, LLM).
- Prozesse fuer ADR Pflege (Review Meetings, Decision Log).
- Tooling Doku fuer CLI, Templates, Automation.
- Export-Funktion fuer Audits (PDF, HTML).

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit Governance ueber Archivierung superseded ADRs.
- Integration in Structurizr DSL fuer C4-Sichten.
- Automatische Synchronisation mit Roadmap Highlights.
- Abhaengigkeit zu Risk Register fuer Decision Impacts.

## Links
- [ADR 0001 LLM Provider Agnostic](md.html?path=adr/ADR-0001-llm-provider-agnostic.md)
- [ADR 0002 RAG vs Context Only](md.html?path=adr/ADR-0002-rag-vs-context-only.md)
- [Architecture C4](md.html?path=architecture/c4.md)
- [Roadmap](md.html?path=roadmap.md)
