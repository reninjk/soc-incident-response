# SOC Escalation Matrix

**Last Updated:** 2026-01-01
**Owner:** SOC Manager
**Review Cycle:** Quarterly

> Use this matrix to determine who to notify, when, and how during any security incident.
> When in doubt — escalate. Over-communicating is always better than under-communicating.

---

## Escalation by Severity

### P1 — Critical (Ransomware, Active Breach, Data Exfiltration)

| Time | Who | How | Message |
|------|-----|-----|---------|
| T+0 (immediate) | SOC Manager | Phone call | "P1 declared: [brief description]. Activating IR." |
| T+0 | IR Team (on-call) | PagerDuty / phone | Automated alert + phone backup |
| T+15 min | CISO | Phone call | "Confirmed P1: [description]. [Impact]. We are [action]." |
| T+30 min | IT Director | Email + phone | Systems potentially affected; containment underway |
| T+1 hr | Legal Counsel | Phone + secure email | If PII/data involved or criminal activity suspected |
| T+1 hr | DPO | Phone + secure email | If personal data involved |
| T+2 hr | CEO / Executive Team | CISO briefing | CISO informs CEO with business impact summary |
| T+4 hr | Cyber Insurance | Email | Notify insurer per policy terms |
| As required | Regulatory body | Per legal guidance | GDPR 72-hr window, NIS2, PCI-DSS, etc. |
| As required | Law enforcement | Phone + formal report | If criminal activity / nation-state confirmed |

### P2 — High (Account Compromise, Malware Spread, Significant Threat)

| Time | Who | How | Message |
|------|-----|-----|---------|
| T+0 | SOC Manager | Teams/Slack + phone if no response | "P2 declared: [description]. L2 analyst [name] leading." |
| T+30 min | IT Security Lead | Email + Teams | Situational awareness; may need support |
| T+1 hr | CISO | Email (phone if no response in 30 min) | Update on P2; expected resolution timeline |
| As needed | Affected business owner | Email | Asset owner of affected system |
| T+4 hr | CISO | Email update | Progress update |

### P3 — Medium (Phishing Attempt, Single Endpoint Malware)

| Time | Who | How | Message |
|------|-----|-----|---------|
| T+0 | L1 → L2 (if needed) | Teams/ticket | Standard escalation within SOC |
| T+4 hr | SOC Manager | Ticket update | FYI in daily summary if notable |
| On resolution | IT Security Lead | Ticket closure note | If affected systems required patching |

### P4 — Low (Policy Violation, Suspicious Log Activity)

| Time | Who | How | Message |
|------|-----|-----|---------|
| During shift | L1 handles | Ticket | Standard investigation, no escalation required |
| If pattern emerges | SOC Manager | Teams | Flag trend for tuning review |

---

## Key Contact Directory

> 🔒 **Sensitive — store actual names/numbers in your secure password manager / on-call system.**
> This template uses role names; populate with real contacts before operational use.

| Role | Name | Phone | Email | Backup |
|------|------|-------|-------|--------|
| SOC Manager | | | | |
| CISO | | | | |
| IT Director | | | | |
| Legal Counsel | | | | |
| Data Protection Officer (DPO) | | | | |
| PR / Communications Lead | | | | |
| CEO | | | | |
| Cyber Insurance (policy number) | | | | |
| EDR Vendor Support | | | | |
| SIEM Vendor Support | | | | |
| Managed SOC Provider (if applicable) | | | | |
| Law Enforcement Contact (cyber unit) | | | | |
| CERT / CSIRT (national) | | | | |
| Forensic Retainer Contact | | | | |

---

## On-Call Rota

| Week | Primary On-Call | Secondary On-Call | Manager On-Call |
|------|----------------|------------------|----------------|
| Current | | | |
| Next | | | |

**On-call expectations (P1):**
- Respond to page within **15 minutes**
- Available to join bridge call within **30 minutes**
- If unreachable after 2 attempts → escalate to secondary

---

## Communication Templates

### Initial P1 Notification (to CISO)
```
Subject: [P1 INCIDENT] [Brief Title] — ACTIVE

Time: [HH:MM UTC]
Incident: [One sentence description]
Current Status: Active investigation underway
Affected Systems: [List]
Potential Impact: [Data/business/user impact]
Actions Taken: [What containment is in progress]
Next Update: In [X] minutes or on significant change

IC: [Name] | Bridge: [Call link/number]
```

### P1 Update (every 30 min)
```
Subject: [P1 UPDATE #X] [Brief Title] — [Status]

Time: [HH:MM UTC]
Progress: [What has been done since last update]
Current Status: [Contained / Eradicating / Recovering]
Remaining Actions: [What's left to do]
ETA to Resolution: [Best estimate]
Blockers: [Any blockers requiring leadership action]

Next update: [Time]
```

### P1 Resolution Notification
```
Subject: [P1 RESOLVED] [Brief Title]

Time Resolved: [HH:MM UTC]
Total Duration: [X hours Y minutes]
Summary: [2–3 sentence description of what happened and how it was resolved]
Impact: [Final impact summary]
Immediate Actions Complete: [List key containment/eradication actions]
PIR Scheduled: [Date/Time]
Incident Report: [Link to ticket/document]

SOC Manager: [Name]
```

---

## Escalation Decision Tree

```
Alert fires
    │
    ▼
Is it a confirmed True Positive?
    ├── NO → Document, tune, close
    └── YES
         │
         ▼
    Severity P3 or P4?
         ├── YES → L1 handles, update ticket, daily summary
         └── NO (P1/P2)
              │
              ▼
         Notify SOC Manager immediately (phone for P1)
              │
              ▼
         Does it involve data / PII / critical systems?
              ├── YES → Notify CISO + Legal within 1 hour
              └── NO → CISO email update within 4 hours
                   │
                   ▼
              Is there evidence of criminal activity?
                   ├── YES → Notify Legal + consider law enforcement
                   └── NO → Continue IR per playbook
```
