---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Compliance Framework

## Zweck & Scope
Dieses Dokument fasst gesetzliche, regulatorische und interne Compliance-Anforderungen zusammen. Es beschreibt Kontrollpunkte, Reporting und Nachweiserbringung fuer die Plattform.

## Enterprise-Anforderungen
- Konformitaet zu GDPR, EU-AI-Act, SOC2, ISO 27001 sowie internen Richtlinien.
- Vollstaendige Audit Trails, Evidence Packs und Kontroll-Reports mit Sign-off.
- Data Protection Impact Assessments (DPIA) fuer alle High-Risk Use Cases.
- Risk-based Access Control, Segregation of Duties und periodische Reviews.
- Automatisierte Compliance Checks in CI/CD (Policy-as-Code, SAST/DAST, License).

## Technologie-Stack
- Governance Tools: Azure Policy, Microsoft Defender for Cloud, Compliance Manager.
- Policy-as-Code: OPA/Gatekeeper, Kyverno, Checkov, Conftest.
- Evidence Management: Neon Compliance Schema, SharePoint Export, GitOps Artefakte.
- Auditing: Loki, Neon, Kafka, Immutable Storage (Object Lock, WORM Buckets).
- Security Testing: Trivy, Gitleaks, Dependency-Track, ZAP, SonarQube.

## Architektur & Betrieb
Compliance Requirements sind in Kontrollkatalogen abgebildet. Pipelines erzeugen Evidence (SBOM, Testberichte, Signaturen). Health Monitor ueberwacht Pflichtmetriken. Das Compliance Board reviewt monatlich KPI und Abweichungen. Automatisierte Tickets fuer Policy Violations gehen an Operations/Security. Evidence wird in Neon versioniert und ueber SharePoint fuer Auditoren bereitgestellt; Sammelpunkte und Checklisten siehe `md.html?path=templates/compliance-evidence.md`. 

## Schnittstellen & Vertraege
- Reporting APIs fuer Auditoren (CSV/JSON, Role-Based Access, mTLS).
- Integration in Governance Dashboard (`md.html?path=governance/governance.md`).
- Webhooks zu Incident Response fuer Breaches und Policy Violations.
- DPIA Templates und Data Mapping Tools fuer neue Features.

## Control Matrix & Evidence Mapping
| Framework Control | Plattform Kontrolle | Evidenz / Quelle |
| ----------------- | ------------------- | ---------------- |
| ISO 27001 A.5 (Policies) | Governance Board, Policy-as-Code Repositories | `md.html?path=governance/governance.md`, GitOps Policy Repo, CAB Minutes |
| ISO 27001 A.8 (Asset Management) | CMDB Sync, Tenant Inventory, Secrets Register | `md.html?path=tenancy/tenancy.md`, Neon `compliance.assets` Schema |
| ISO 27001 A.12 / SOC2 CC5 (Operations) | Runbooks, Change Management, OnCall | `md.html?path=operations/operations.md`, ServiceNow Change Tickets |
| ISO 27001 A.16 / SOC2 CC7 (Incident) | Incident Response Playbooks, Health Monitor Alerts | `md.html?path=operations/operations.md`, Alertmanager Audit Logs |
| ISO 27018 / GDPR Art. 32 (Data Protection) | Data Classification, Encryption, Access Reviews | `md.html?path=data/data.md`, Vault Audit, Access Review Reports |
| GDPR Art. 35 (DPIA) | DPIA Register, RAG Context Controls | DPIA SharePoint Library, `md.html?path=risk/risk.md` |
| EU AI Act Title III | Model Risk Classification, Human-in-the-Loop Controls | `md.html?path=goal/goal.md`, `md.html?path=evaluation/evaluation.md`, ADR-0002 |
| SOC2 CC2 (Governance) | Quarterly Compliance Steering, Risk Register Integration | Board Minutes, `md.html?path=governance/governance.md`, `md.html?path=risk/risk.md` |
| SOC2 CC6 (Change Mgmt) | GitOps, Signed Releases, CAB Approval | `md.html?path=deployment/deployment.md`, Cosign Logs, CAB Tickets |

## Deliverables
- Compliance Control Matrix (Mapping zu Services, Owner, Status) mit quartalsweiser Aktualisierung.
- Jahresplan fuer Audits, PenTests, DPIAs mit Abnahme durch Governance Board.
- Evidence Pack Templates und Automations (CI/CD Export, SharePoint Publishing).
- Schulungsmaterial fuer Entwickler (Secure Coding, Data Handling, AI Risk).
- Compliance KPI Dashboard mit Trendanalysen (Policy Coverage, Audit Score, Overdue Actions).

## Offene Punkte & Abhaengigkeiten
- Finalisierung EU-AI-Act Klassifizierung pro Agenten Capability.
- Abstimmung mit Risk Register ueber Risikoakzeptanzen und Aggregation.
- Abstimmung mit Legal ueber Cross-Border Data Transfers und Standard Clauses.
- Abhaengigkeit zu Security und Governance fuer Policy Updates und Reviews.

## Links
- [Security Framework](md.html?path=security/security.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Risk Register](md.html?path=risk/risk.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
