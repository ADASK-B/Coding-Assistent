---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Operations Runbooks

## Zweck & Scope
Operations verantwortet die 24/7 Stabilitaet der Plattform. Dieses Dokument sammelt Runbooks, Eskalationspfade und Maintenance Prozesse.

## Enterprise-Anforderungen
- OnCall Abdeckung mit dokumentierten SLA/SLO, Escalation Policies, Handover.
- Vollstaendige Runbooks fuer Incidents, Changes, Maintenance, Disaster Recovery.
- Integrationen mit Monitoring, Ticketing, Compliance und FinOps.
- Nachvollziehbare Postmortems, Lessons Learned, KPI Tracking.
- Sicherheits- und Datenschutzrichtlinien fuer Betriebszugriffe (Least Privilege, JIT).

## Technologie-Stack
- Incident Tools: PagerDuty, ServiceNow, Microsoft Teams, Status Page.
- Monitoring: Health Monitor, Prometheus, Grafana, Loki, Alertmanager, Tempo.
- Automation: Ansible, Terraform, n8n, PowerShell/Bash Scripts.
- Knowledge Base: Docs Portal, Confluence, GitOps Repositories.
- Ticketing: Azure DevOps Boards, ServiceNow Change/Problem Module.

## Architektur & Betrieb
Operations arbeitet aus dem Control-Plane Namespace. Runbooks definieren Owner, Vorbedingungen, Schritte, Erfolgsindikatoren, Rollback. Automatisierte Playbooks via n8n/Ansible, manuelle Schritte mit Checklisten. Incident Reviews verknuepfen Monitoring und Root-Cause Analysen. Zugriff erfolgt ueber Bastion mit MFA und JIT-Zugriff; Aktionen sind in Neon Audit-Tables protokolliert.

## Schnittstellen & Vertraege
- Incident Tickets muessen Monitoring IDs, Tenant, Impact und Owner enthalten.
- Change Requests referenzieren Deployment Strategy, SLO Impact und Governance Policies.
- Kommunikations-Protokoll: Stakeholder Updates (Teams) und Kunden-Benachrichtigung (Status Page).
- Integration mit Compliance (Evidence) und Risk (Impact Bewertung).

## Standard Runbooks
Die folgenden Runbooks nutzen standardisierte Templates aus `md.html?path=templates/postmortem.md` und `md.html?path=templates/change-request.md`.
### P1 Incident (Service Outage)
1. **Triage (PagerDuty Incident Commander)**
   - Health Monitor Check, Alertmanager Payload, betroffene Tenants notieren.
2. **Kommunikation (Comms Lead)**
   - Status Page (P1 Template) innerhalb 10 Minuten, Stakeholder-Chat starten.
3. **Diagnose (Responder)**
   - Prometheus Query Pack ausfuehren, Loki Log Filter anwenden, letzte Changes pruefen (`git log`, Change Ticket).
4. **Mitigation (Responder)**
   - Runbook Schritt (Rollback Helm Release, Scale Down Queue, Switch to Standby Neon Branch).
5. **Verifikation (Ops + QA)**
   - Health Monitor Gruen, Synthetic Tests `make verify-prod` gruen, SLO Burn Rate unter 2x.
6. **Closure (Incident Commander)**
   - Incident Ticket schliessen, Postmortem Template aus `quality/quality.md` laden, Due Dates setzen.

**SLA**: Acknowledge < 5 min, Mitigation < 30 min, Resolver < 60 min. **KPIs**: MTTA, MTTR, Customer Updates puenktlich.

### Planned Change (Deployment)
1. Change Request in ServiceNow + ADO Pull Request (verlinkte Tickets).
2. CAB/Governance Approval mit Verweis auf `deployment/deployment.md`.
3. Pre-Checks: SLO Scorecard (Grafana), Error Budget, Capacity Baseline.
4. Durchf?hrung: Argo CD Sync (Progressive Rollout), Live Monitoring Dashboard.
5. Post-Checks: Health Monitor OK, Alertmanager quiet, FinOps Ledger Update.
6. Abschluss: Change Record Dokumentieren, Evidence Pack (`compliance` SharePoint) aktualisieren.

### Secrets Rotation (Vault/ESO)
1. Rotation Request durch Security (Jira) mit Klassifizierung.
2. Ops generiert neues Secret via Vault Role, speichert Evidence in Neon Audit.
3. GitOps SecretStoreRef Update, Argo Sync, Drift Check.
4. Smoke Tests fuer betroffene Services, PagerDuty Quiet Window bestaetigen.
5. Altes Secret nach X Minuten revoked, Access Review aktualisieren.

## Deliverables
- Runbook Sammlung (Incident, Recovery, Maintenance, Security Events) mit Versionierung.
- OnCall Schedule, Escalation Chains, Contact Lists (PagerDuty Export, Teams Channel).
- Postmortem Template, SLO Breach Prozess, KPI Dashboard.
- Automation Scripts Repository mit Tests und Signaturen.
- Evidence Pack fuer Audits (Logs, Tickets, Playbook Ausfuehrung, Approvals).

## Offene Punkte & Abhaengigkeiten
- Continuous Training fuer Ops Engineers (LLM Tools, Observability, Compliance).
- Integration mit SLO Dashboard fuer automatische KPI Updates.
- Abstimmung mit Security fuer Incident Handling und Secret Rotation.
- Abhaengig von Governance fuer Change Approval Prozesse und Policy Updates.

## Links
- [Monitoring & Observability](md.html?path=monitoring/monitoring.md)
- [Disaster Recovery](md.html?path=dr/dr.md)
- [Governance Leitfaden](md.html?path=governance/governance.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [SLO Framework](md.html?path=slo/slo.md)
