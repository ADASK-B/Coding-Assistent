# Security Framework

## Zweck & Scope
Dieses Dokument beschreibt Sicherheitsprinzipien, Kontrollen und Betriebsprozesse fuer die Plattform. Es umfasst Identitaet, Zugriff, Secrets, Supply-Chain und Detektion entlang des gesamten Agenten-Lebenszyklus.

## Enterprise-Anforderungen
- Zero-Trust-Architektur mit Least-Privilege, Just-in-Time Credentials und zentralem Audit.
- EU-AI-Act, GDPR und SOC2 konforme Prozesse inklusive DPIA und Model Risk Management.
- Durchgaengige Verschluesselung (in transit und at rest) fuer Neon, Redis und Artefakte.
- Signierte Build- und Deployment-Pipelines mit Policy Gates (OPA/Gatekeeper, Kyverno).
- Tamper-Evident Logging, Incident Response Runbooks und SIEM-Integration.

## Technologie-Stack
- Identity Provider: Entra ID / Keycloak, Workload Identity fuer K8s.
- Secrets: HashiCorp Vault, Azure Key Vault, External Secrets Operator.
- Policy Engines: OPA/Gatekeeper, Kyverno, Cerbos.
- Supply Chain: Cosign, Gitleaks, Syft/SBOM, Trivy/Grype, DefectDojo.
- Detection: Falco, Aqua Trivy Operator, Prometheus Alerting, Loki Rules.

## Architektur & Betrieb
Security Controls sind in Schichten organisiert: Perimeter (Traefik/WAF), Cluster Policies, Workload Policies, Data Controls. GitOps erzwingt signierte Commits und Policy-Pruefungen. Secrets werden kurzlebig ueber Vault Dynamic Secrets erzeugt. Audit Logs landen ueber Kafka in Neon/SIEM, Health Monitor prueft Security Heartbeats.

## Schnittstellen & Vertraege
- OAuth2/OIDC fuer User- und Service-Login (Scopes pro Tenant & Rolle).
- Admission Controller (OPA/Kyverno) akzeptieren nur signierte Images und Policies.
- Secrets-Injection via ESO (Contract: SecretStoreRef, Rotation Interval, Policy).
- Webhooks fuer Alertmanager, Defender/Sentinel und Incident-Response.
- Reporting-Schnittstellen zu Compliance (DPIA, Risk Register, Evidence Packs).

## Deliverables
- Security Architecture Diagramme, Threat Model, Controls Matrix.
- Policy-as-Code Repositories mit Tests (Conftest, Kyverno CLI).
- Incident Response Plan, Breach Notification Template, Security Runbooks.
- Secret Management Playbook (Rotation, Revocation, Access Reviews).

## Offene Punkte & Abhaengigkeiten
- Entscheidung ueber zentralen SIEM (Azure Sentinel vs. Splunk).
- Abstimmung mit Governance zu Approval-Workflows fuer Secrets/Policies.
- Abhaengigkeit zu Operations fuer Incident Handling und Runbooks.
- Integration mit Evaluation fuer Modell-Sicherheits-Tests.

## Links
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Compliance Framework](md.html?path=compliance/compliance.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Risk Register](md.html?path=risk/risk.md)
