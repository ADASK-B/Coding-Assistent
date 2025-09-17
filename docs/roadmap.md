# Roadmap

## Zweck & Scope
Roadmap bietet Ueberblick ueber anstehende Initiativen, Releases und Meilensteine. Sie verbindet strategische Ziele mit operativen Deliverables.

## Enterprise-Anforderungen
- Quartalsweise Planung mit genehmigten Budgets und Kapazitaeten.
- Transparente Abhaengigkeiten zu Risk, Compliance, Security, Operations.
- Verfolgung von KPIs (Delivery Rate, SLA Ziele, Cost Targets).
- Integration mit Azure DevOps, Governance Board und Public Docs.
- Regelmaessige Review- und Update-Zyklen (Monat/Q, CAB).

## Technologie-Stack
- Azure DevOps Roadmaps/Boards, GitHub Projects (optional), Mural/Miro fuer Workshops.
- Reporting ueber PowerBI, Grafana Dashboards.
- n8n Automationen fuer Statusaggregation (Progress, Risks, Blocker).
- Documentation hier im Docs Portal.

## Architektur & Betrieb
Roadmap Eintraege folgen Template (Initiative, Outcome, KPI, Dependencies, Owner). Status wird woechentlich aktualisiert. Governance Meeting nutzt Roadmap fuer Entscheidungen. Integrationen mit Risk Register, SLO Dashboard und Evaluation liefern Signale fuer Priorisierung.

## Schnittstellen & Vertraege
- Roadmap API `/api/v1/roadmap` fuer Sync mit externen Tools.
- Links zu ADRs, Risk Items, Compliance Tasks.
- Reporting an Management (OKRs) und Kunden (QBRs).
- Feedback Kanal fuer Stakeholder (Teams, Portale).

## Deliverables
- Quartals-Roadmap mit Themes (z. B. Q1 Core Hardening, Q2 Multi-Tenant, Q3 RAG).
- Release Plan inkl. Feature Freeze, Launch Prep, Communication Kits.
- Statusberichte (weekly digest, monthly executive summary).
- Backlog fuer Ideen/Research mit Bewertungsmatrix.

## Offene Punkte & Abhaengigkeiten
- Synchronisation mit Product Teams und Kundenvorhaben.
- Integration in Evaluation fuer Erfolgsmessung (Outcome KPIs).
- Aufbau eines Public Status Dashboards fuer Kunden.
- Abhaengig von Governance fuer Priorisierung und Budget.

## Links
- [ADR Index](md.html?path=adr/README.md)
- [Risk Register](md.html?path=risk/risk.md)
- [SLO Framework](md.html?path=slo/slo.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)

## Aktuelle Meilensteine
- **Q4 2025**: RAG Rollout (ADR-0002), Multi-Tenant Automation GA, Security Audit.
- **Q1 2026**: Kubernetes Production Cutover, DR GameDay, Evaluation v2 Launch.
- **Q2 2026**: External API GA, Compliance Certification, Cost Optimization Sprint.
