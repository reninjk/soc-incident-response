# Alert Triage Runbook

**Owner:** SOC Analyst (L1/L2)
**SLA:** Begin triage within 15 min of alert
**Last Updated:** 2026-05

---

## Purpose
Standardized procedure for triaging all incoming SIEM/EDR/email alerts to determine validity, severity, and required response action.

## Step 1: Alert Intake
1. Open the alert from the SIEM/ticketing queue
2. Record: Alert name, timestamp, source system, affected asset(s)
3. Assign the alert to yourself in the ticketing system
4. Note the alert rule/signature that fired

## Step 2: Context Gathering
Collect the following before making any decision:

| Data Point | Where to Find |
|------------|---------------|
| User account details | AD / Identity Provider |
| Host details (OS, owner, criticality) | CMDB / Asset inventory |
| Recent logins / activity | SIEM / EDR |
| Network connections at time of alert | SIEM / NDR |
| Threat intel on IOCs | TI platform / VirusTotal |
| Prior alerts on same user/host (7d) | SIEM correlation |

## Step 3: Determination Matrix

| Alert Verdict | Criteria | Action |
|---------------|----------|--------|
| **True Positive** | Malicious activity confirmed | Open IR ticket, follow playbook |
| **Likely True Positive** | Strong indicators, needs verification | Escalate to L2, collect more evidence |
| **False Positive** | Benign activity explains alert | Close with notes, tune rule if recurring |
| **Undetermined** | Insufficient data | Escalate to L2 with all collected context |

## Step 4: Severity Assignment
Refer to the severity matrix:
- **P1 Critical:** Active breach, ransomware, data exfiltration in progress
- **P2 High:** Account compromise, malware with C2 comms, lateral movement
- **P3 Medium:** Phishing click, single endpoint malware, policy violation
- **P4 Low:** Suspicious but explainable, informational

## Step 5: Escalation Decision

**Escalate to L2 if:**
- You cannot determine true/false positive within 30 minutes
- Alert severity is P1 or P2
- Multiple users or systems are involved
- C2 communication is detected
- Data exfiltration indicators are present

**Close as False Positive if:**
- Activity is explained by a known change or authorized action
- No malicious indicators found after full context review
- Verified with asset owner/user

## Step 6: Documentation
Every alert (regardless of outcome) must be documented with:
- [ ] Initial alert details
- [ ] Context gathered
- [ ] Decision and rationale
- [ ] Actions taken
- [ ] Rule tuning recommendation (if false positive)
- [ ] Time to triage, time to close

## Common False Positive Scenarios
| Alert Type | Common FP Cause |
|------------|-----------------|
| Brute force login | User forgot password |
| Unusual geo login | VPN / travel |
| Port scan | Vulnerability scanner (scheduled) |
| Large data transfer | Authorized backup job |
| Admin tool usage | IT admin activity |

---
*Escalation path: L1 → L2 → Senior Analyst → SOC Manager*
