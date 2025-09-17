---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Compliance Evidence Checklist

## Meta
- **Control ID / Framework**: (ISO 27001 / SOC2 / GDPR / EU AI Act)
- **Evidence Owner**:
- **Period**: Q#/YYYY
- **Review Date**:

## Evidence Inventory
| Control | Artefakt | Speicherort | Reviewer | Status |
| ------- | -------- | ----------- | -------- | ------ |
| | | SharePoint/ | | |

## Automated Artefakte
- SBOM Export (`pipeline-artifacts/sbom-<date>.json`)
- Vulnerability Scan Reports (`trivy/<date>.html`)
- Policy-as-Code Evaluations (`conftest/<date>.json`)
- Signed Release Manifeste (`cosign bundle`)

## Manual Artefakte
- Meeting Minutes (Governance Board, CAB)
- Access Review Reports (Neon, Vault)
- DPIA Dokumente / AI Risk Assessments
- Training & Awareness Nachweise

## Validation Steps
1. Verifiziere Signaturen / Hashes
2. Pr?fe Zeitstempel (innerhalb Beobachtungsperiode)
3. Best?tige Reviewer Unterschrift
4. Speichere im Evidence Store (SharePoint + Neon)
5. Aktualisiere Control Matrix

## Exceptions & Waivers
- Beschreibung, Laufzeit, Genehmiger
- Mitigations-Plan

## Sign-off
- Compliance Lead:
- Security Lead:
- External Auditor (falls erforderlich):

## Links
- [Compliance Framework](md.html?path=compliance/compliance.md)
- [Security Framework](md.html?path=security/security.md)
- [Risk Register](md.html?path=risk/risk.md)
