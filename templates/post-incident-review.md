# Post-Incident Review (PIR) Template

> **Blameless post-incident reviews focus on systems and processes, not individuals.**
> Complete within 72 hours of incident closure for P1/P2; within 1 week for P3.

---

## Incident Summary

| Field | Details |
|-------|---------|
| Incident ID | |
| Incident Title | |
| Severity | P1 / P2 / P3 |
| Detection Date/Time | |
| Resolution Date/Time | |
| Total Duration | |
| MTTD (Mean Time to Detect) | |
| MTTR (Mean Time to Respond) | |
| Review Date | |
| Review Facilitator | |
| Attendees | |

---

## What Happened — Executive Summary

> *3–5 sentence non-technical summary suitable for leadership. What happened, what was the impact, how was it resolved.*

---

## Detailed Incident Timeline

| Date/Time (UTC) | Event | Who | Source |
|-----------------|-------|-----|--------|
| | Initial indicator observed | | |
| | Alert fired in SIEM | | |
| | Analyst began investigation | | |
| | Incident declared | | |
| | Escalation to L2/Manager | | |
| | Containment action taken | | |
| | Eradication confirmed | | |
| | Recovery completed | | |
| | Incident closed | | |

---

## Impact Assessment

| Impact Area | Details |
|-------------|---------|
| Systems/Services Affected | |
| Users/Accounts Affected | |
| Data Affected (type & volume) | |
| Business Operations Impact | |
| Customer Impact | |
| Financial Impact (estimated) | |
| Reputational Impact | |
| Regulatory Impact | |
| Downtime Duration | |

---

## Root Cause Analysis

### Direct Cause
> *What immediately caused the incident? (e.g., unpatched vulnerability exploited, misconfigured firewall rule)*

### Contributing Factors
> *What conditions allowed the direct cause to have this impact?*

1.
2.
3.

### Root Cause
> *The fundamental systemic reason the incident could occur.*

### 5-Why Analysis

| Why | Answer |
|-----|--------|
| Why did the incident occur? | |
| Why did that happen? | |
| Why did that happen? | |
| Why did that happen? | |
| Why did that happen? (root cause) | |

---

## What Went Well

> *Be specific — what detection, response, or communication actions were effective?*

1.
2.
3.

---

## What Could Be Improved

> *Be specific — what slowed detection, response, or recovery?*

1.
2.
3.

---

## Detection Analysis

| Question | Answer |
|----------|--------|
| How was the incident detected? | |
| Was detection automated or manual? | |
| Were there earlier indicators that were missed? | |
| What was the lag between first indicator and alert? | |
| Were existing detection rules effective? | |
| Were any rules triggered that proved to be FPs? | |

---

## Response Analysis

| Question | Answer |
|----------|--------|
| Was the correct playbook followed? | |
| Were escalation thresholds appropriate? | |
| Were containment actions effective and timely? | |
| Did tooling function as expected? | |
| Were communication channels adequate? | |
| Were staffing levels sufficient? | |

---

## Action Items

> All action items must have an owner and a due date. Track in your ticketing system.

| # | Action Item | Category | Owner | Due Date | Priority | Status |
|---|-------------|----------|-------|----------|----------|--------|
| 1 | | Detection | | | High | Open |
| 2 | | Process | | | High | Open |
| 3 | | Tooling | | | Medium | Open |
| 4 | | Training | | | Medium | Open |
| 5 | | Policy | | | Low | Open |

**Categories:** Detection | Response | Tooling | Process | Training | Policy | Infrastructure | Vendor

---

## Metrics

| Metric | This Incident | Team Average | Target |
|--------|--------------|-------------|--------|
| MTTD | | | < 15 min |
| MTTR | | | Per SLA |
| False positive rate for triggering alerts | | | < 30% |
| Playbook adherence score (1–5) | | | 4+ |
| Communication timeliness score (1–5) | | | 4+ |

---

## Sign-off

| Role | Name | Date | Signature |
|------|------|------|-----------|
| Incident Commander | | | |
| SOC Manager | | | |
| CISO (P1 only) | | | |

---

*This document is CONFIDENTIAL. Distribute only to those with need-to-know.*
