---
Owner: Arthur Schwan
Last-Reviewed: 2025-09-17
Next-Review: 2025-12-01
Version: 1.0
Status: Approved
---
# Major Incident & Postmortem Template

## Incident Header
- **Incident ID**: `INC-YYYYMMDD-###`
- **Severity**: (P1/P2/P3)
- **Start Time** / **End Time** / **Duration**
- **Services / Tenants**:
- **Impact Summary**:
- **Incident Commander**:
- **Root Cause Category**: (People/Process/Technology/External)

## Timeline (UTC)
| Time | Actor | Event |
| ---- | ----- | ----- |
| HH:MM | | |

## Detection & Response
- **Detection Method**: (Alertmanager, Health Monitor, Customer Ticket)
- **Alert IDs / Links**:
- **Runbooks Executed**:
- **Mitigation Actions**:

## Customer & Stakeholder Communication
- Status Page Updates (timestamps/links)
- Direct Notifications (Tenants, Execs)

## Root Cause Analysis
- Problem Statement
- Contributing Factors
- Why Ladder (5-Whys oder Fishbone)
- Evidence Links (Logs, Metrics, Screenshots)

## Corrective & Preventive Actions
| Action | Owner | Due Date | Status |
| ------ | ----- | -------- | ------ |

## SLO / Error Budget Impact
- Burn Rate (before/after)
- Credits / Penalties

## Compliance & Governance
- Evidence stored at: `SharePoint/Compliance/INC-...`
- Follow-up Audit Required? (Yes/No)
- Lessons Learned approved by Governance Board (Date)

## Sign-Off
- Incident Commander:
- SRE Lead:
- Governance Representative:

## Links
- [Operations Runbooks](md.html?path=operations/operations.md)
- [SLO Framework](md.html?path=slo/slo.md)
- [Risk Register](md.html?path=risk/risk.md)
