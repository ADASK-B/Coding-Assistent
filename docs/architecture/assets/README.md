---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Architecture Assets

## Zweck & Scope
Dieses Verzeichnis enthaelt die generierten Diagramme und Quellbeschreibungen fuer die C4 Sichten der Plattform.

## Struktur
- `system-context.mmd`: Kontextsicht in Mermaid/C4-Notation.
- `container.mmd`: Containerdarstellung der Plattformservices.
- `gateway-components.mmd`: Komponentenaufteilung des Gateways.
- `draft-pr-sequence.mmd`: Sequence Diagram fuer Draft-PR Flow.
- `deployment.mmd`: Deployment View (AKS, Managed Services).

## Pflegeprozess
1. ?nderungen am Structurizr DSL (`architecture/structurizr.dsl`) durchf?hren.
2. `npm run c4:export` ausfuehren, wodurch PNG/SVG generiert werden.
3. Exportierte Dateien unter `docs/architecture/assets/export/` ablegen (Git tracked).
4. Hash-Werte in Neon `architecture.assets` speichern, Review-Protokoll in Governance Board Dokumentation aktualisieren.
5. Pull Request inklusive Screenshot/PNG-Anhang und Reviewer (Architecture Board).

## Hinweise
- Mermaid-Dateien dienen als human-readable Referenz und koennen mit `mmdc` in PNG ueberfuehrt werden.
- Alle Diagramme folgen den Namenskonventionen `c4-<view>-<version>.png`.
- Reviews erfolgen mindestens vierteljaehrlich oder nach wesentlichen Architekturentscheidungen.

## Links
- [C4 Dokumentation](md.html?path=architecture/c4.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [ADR Index](md.html?path=adr/README.md)
