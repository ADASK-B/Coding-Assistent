# Quick Setup Guide

## Zweck & Scope
Diese Anleitung fuehrt neue Teams durch die initiale Einrichtung der Plattform fuer lokale Entwicklungen und Testdeployments.

## Enterprise-Anforderungen
- Verifizierbarer Setup-Prozess mit Checks fuer Security, Compliance und Observability.
- Minimale Zeit bis zur lauffaehigen Umgebung (<30 Minuten) bei reproduzierbaren Ergebnissen.
- Automatisierte Provisionierung ueber Skripte/GitOps ohne manuelles Klicken.
- Integration in Access Management und Audit Logging.
- Dokumentierte Erfolgsbedingungen (Smoke Tests, Health Checks).

## Technologie-Stack
- Prereqs: Docker Desktop/Podman, Node 20, Python 3.11, Make/Taskfile.
- Tooling: Azure CLI, kubectl, helm, terraform, n8n CLI.
- GitOps Repos fuer Compose/K8s, Secrets via Vault CLI oder SOPS.
- Observability Stack (Prometheus, Grafana, Loki) optional via Compose Profile.

## Architektur & Betrieb
Setup Script `scripts/setup.ps1` prueft lokale Dependencies, installiert CLI Tools und kopiert Beispiel-Konfigurationen. Docker Compose startet Kernservices (Gateway, Orchestrator, Redis, Neon Proxy, Monitoring). Health Monitor validiert Ports. Optionaler K8s Setup nutzt Kind/Helm fuer lokalen Cluster.

## Schnittstellen & Vertraege
- Requires Zugriff auf Git Repos, Container Registry, Vault Secrets, Neon Branch.
- Setup Logging schreibt nach `logs/setup` und an Health Monitor.
- Access-Requests fuer Entra/Keycloak via Self-Service (Ticket Template).
- Smoke-Test Contract: `make verify-local` muss gruen sein.

## Deliverables
- Checklist fuer lokale Installation inkl. Troubleshooting.
- Scripts (PowerShell/Bash) fuer Windows/Mac/Linux.
- Sample Configs (`config/local/*.yaml`) mit Hinweis auf Secrets.
- Onboarding FAQ fuer Developer.

## Offene Punkte & Abhaengigkeiten
- Konsolidierung der Setup Scripts (PowerShell vs. Bash).
- Integration mit Operations fuer Supportprozesse.
- Abstimmung mit Security ueber lokale Secret-Handling Policies.
- Aktualisierung bei neuen Services (LLM Router, Health Monitor).

## Links
- [System Requirements](md.html?path=requirements/requirements.md)
- [Deployment Strategy](md.html?path=deployment/deployment.md)
- [Operations Runbooks](md.html?path=operations/operations.md)
- [Roadmap](md.html?path=roadmap.md)
