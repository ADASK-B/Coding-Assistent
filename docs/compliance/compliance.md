# Compliance Framework

## Zweck & Scope
Dieses Dokument fasst gesetzliche, regulatorische und interne Compliance-Anforderungen zusammen. Es beschreibt Kontrollpunkte, Reporting und Nachweiserbringung fuer die Plattform.

## Enterprise-Anforderungen
- Konformitaet zu GDPR, EU-AI-Act, SOC2, ISO 27001, branchenspezifischen Richtlinien.
- Vollstaendige Audit Trails, Evidence Packs und Kontroll-Reports.
- Data Protection Impact Assessments (DPIA) fuer alle High-Risk Use Cases.
- Risk-based Access Control, SoD (Segregation of Duties) und Periodic Reviews.
- Automatisierte Compliance Checks in CI/CD (Policy-as-Code, SAST/DAST, License).

## Technologie-Stack
- Governance Tools: Azure Policy, Defender for Cloud, Compliance Manager.
- Policy-as-Code: OPA/Gatekeeper, Kyverno, Checkov, Conftest.
- Evidence Management: Neon Compliance Schema, SharePoint/Confluence Export.
- Auditing: Loki, Neon, Kafka, Immutable Storage (Object Lock).
- Security Testing: Trivy, Gitleaks, Dependency-Track, Zaproxy.

## Architektur & Betrieb
Compliance Requirements sind in Kontrollkatalogen abgebildet. Pipelines erstellen Evidence (SBOM, Testberichte). Health Monitor ueberwacht Pflichtmetriken. Compliance Board reviewt monatlich KPI und Abweichungen. Automatisierte Tickets fuer Policy Violations gehen an Operations/Security.

## Schnittstellen & Vertraege
- Reporting APIs fuer Auditoren (CSV/JSON, Role-Based Access).
- Integration in Governance Dashboard (`md.html?path=governance/governance.md`).
- Webhooks zu Incident Response fuer Breaches.
- DPIA Templates und Data Mapping Tools fuer neue Features.

## Deliverables
- Compliance Control Matrix (Mapping zu Services, Owner, Status).
- Jahresplan fuer Audits, PenTests, DPIAs.
- Evidence Pack Templates und Automations.
- Schulungsmaterial fuer Entwickler (Secure Coding, Data Handling).

## Offene Punkte & Abhaengigkeiten
- Finalisierung EU-AI-Act Klassifizierung pro Agenten Capability.
- Abstimmung mit Risk Register ueber Risikoakzeptanzen.
- Abstimmung mit Legal ueber Cross-Border Data Transfers.
- Abhaengigkeit zu Security und Governance fuer Policy Updates.

## Links
- [Security Framework](md.html?path=security/security.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Risk Register](md.html?path=risk/risk.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
