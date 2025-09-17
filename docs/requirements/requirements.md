# System Requirements

## Zweck & Scope
Dieses Dokument definiert technische Mindestanforderungen fuer lokale Entwicklung, CI/CD Runner und Produktionsbetrieb.

## Enterprise-Anforderungen
- Verbindliche Hardware-/Software-Spezifikationen mit Nachweisbarkeit.
- Kompatibilitaet mit Sicherheitsrichtlinien (Hardening, Patching, Antivirus).
- Mandantenfaehige Ressourcenplanung (Capacity, Storage, Network).
- Skalierbarkeit fuer >100 parallele Agenten-Fluesse.
- Regelmaessige Review-Zyklen mit Governance/GPO.

## Technologie-Stack
- Developer Workstations: 8+ Cores, 16GB RAM, 100GB SSD, Windows 11/Ubuntu 22.04, WSL2 optional.
- CI Runner: Linux x86_64, 16 Cores, 32GB RAM, GPU optional (vLLM Testing).
- Production: AKS Standard D/E-Series, GPU-Pools (NVIDIA A10/A100) fuer LLM.
- Storage: Premium SSD, Azure Files, S3/Blob fuer Artefakte.
- Network: ExpressRoute/VPN, Firewall Allowlists, DNS via Azure DNS.

## Architektur & Betrieb
Baseline Images enthalten Hardening (CIS Benchmarks), EDR, Logging Agent. Golden AMIs werden via Packer gepflegt. Capacity Planning laeuft ueber Grafana/Prometheus, AutoScaling via KEDA/HPA. Secrets verteilt via Vault; Compliance checkt Patch-Level monatlich.

## Schnittstellen & Vertraege
- Procurement & ITSM Prozesse fuer Bereitstellung (ServiceNow Tickets).
- Monitoring Hook zu SLO Dashboard fuer Auslastung.
- Asset Management CMDB Synchronisation.
- Security Vertraege fuer Hardening Scripts, Patch Windows, Vulnerability Reports.

## Deliverables
- Requirements Matrix (Dev, CI, Prod) inkl. Approval.
- Golden Image Dokumentation und Build Scripts.
- Capacity & Forecast Reports (Quartal).
- Checklisten fuer Hardware Rollout und Onboarding.

## Offene Punkte & Abhaengigkeiten
- Abstimmung mit FinOps ueber GPU Kapazitaetsplanung.
- Integration mit Risk Register fuer Kapazitaets-Engpaesse.
- Entscheidung ueber BYOD vs. Managed Devices fuer externe Teams.
- Abhaengigkeit zu Security fuer Hardening Automatisierung.

## Links
- [Setup Guide](md.html?path=setup/setup.md)
- [Kubernetes Migration](md.html?path=k8s/k8s.md)
- [SLO Framework](md.html?path=slo/slo.md)
- [Risk Register](md.html?path=risk/risk.md)
