# Glossary

## Zweck & Scope
Das Glossar erklaert zentrale Begriffe der Plattform, um gemeinsame Sprache fuer Architektur, Betrieb und Governance sicherzustellen.

## Enterprise-Anforderungen
- Aktualisierte, versionierte Begriffsliste mit Owner und Review Zyklus.
- Mapping zu Compliance, Security und Trainings.
- Mehrsprachigkeit optional (DE/EN) fuer Kundenkommunikation.
- Verlinkung zu ADRs, Runbooks, Policies.
- Integration in Docs Portal mit Suchfunktion.

## Technologie-Stack
- Markdown Glossar mit Linkable Anchors.
- Optionaler Export nach JSON fuer Docs Suche.
- Governance Workflow in Azure Boards fuer Updates.
- Search Index ueber Lunr/Algolia (Future Work).

## Architektur & Betrieb
Begriffe werden in alphabetischer Reihenfolge gepflegt. Aenderungen laufen durch Governance Review. Health Monitor prueft Broken Links. Glossar wird bei Release Zyklen geprueft.

## Schnittstellen & Vertraege
- Beitragsprozess via Pull Request mit Review durch Docs Owner.
- Linkpflicht in ADRs, Runbooks und Compliance Berichten.
- API Idee: `/api/v1/glossary` fuer Integrationen.

## Deliverables
- Alphabetische Liste mit Definition, Kontext, Referenzen.
- Mapping Tabelle (Begriff -> Dokument -> Owner).
- Release Notes fuer Glossar Updates.
- Schulungsunterlagen fuer Onboarding.

## Offene Punkte & Abhaengigkeiten
- Automatisierte Broken Link Detection fuer Glossar-Referenzen.
- Optionale Uebersetzungen fuer Kundenprojekte.
- Integration mit Roadmap fuer neue Begriffe.
- Abhaengig von Governance fuer Review.

## Links
- [Risk Register](md.html?path=risk/risk.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [Roadmap](md.html?path=roadmap.md)

## Begriffe
- **Agent Gateway**: API Eingang, authentifiziert und orchestriert Agentenauftraege.
- **Orchestrator**: Steuert Agentenschritte, Workflows, Kontextmanagement.
- **LLM Router**: Entscheidet ueber Modellanbieter basierend auf Policies.
- **Health Monitor**: Service fuer Synthetic Checks, Status API, Escalations.
- **Error Budget**: Prozentsatz akzeptabler SLO-Verstoesse pro Zeitraum.
- **Tenant**: Mandant mit isolierten Ressourcen, Daten und Policies.
- **Golden Test**: Referenztest zur Bewertung von AI-generierten Ergebnissen.
- **DPIA**: Datenschutz-Folgenabschaetzung nach GDPR.
- **GitOps**: Declarative Infrastructure Management ueber Git und Automatisierung.
- **PITR**: Point in Time Recovery fuer Datenbanken.
