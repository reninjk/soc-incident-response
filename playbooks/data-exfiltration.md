# Playbook: Data Exfiltration

**Severity:** P1 (Critical)
**Last Reviewed:** 2026-01-01
**Owner:** SOC Manager
**MITRE ATT&CK:** T1041 (Exfil over C2), T1048 (Exfil over Alt Protocol), T1567 (Exfil to Cloud), T1030 (Data Transfer Size Limits)

---

## Overview

Data exfiltration is the unauthorised transfer of data outside the organisation. This playbook covers detection through to legal notification. Given the potential for regulatory impact, the CISO and Legal must be notified immediately upon confirmation.

---

## Detection Sources

| Source | Indicator |
|--------|-----------|
| DLP | Large file transfer to personal email / cloud storage |
| SIEM | Unusually large outbound data volume (> baseline × 3) |
| SIEM | Bulk archive creation (zip/tar/rar) followed by outbound transfer |
| Proxy / Firewall | DNS queries to newly registered or rare domains |
| Proxy / Firewall | Large HTTPS POST requests to cloud storage (Dropbox, Mega, WeTransfer) |
| EDR | Staging directory created with sensitive file aggregation |
| CASB | Bulk download from SharePoint / Google Drive |
| Email Gateway | Mass email with large attachments to external addresses |
| User Report | Employee reports unexpected file access by colleague |

---

## Severity Classification

| Scope | Severity |
|-------|----------|
| Internal data, no PII, contained | P2 |
| PII / PHI involved | P1 — regulatory notification likely required |
| IP / trade secrets | P1 |
| Customer data | P1 |
| Confirmed exfiltration to threat actor | P1 |
| Ransomware double-extortion scenario | P1 |

---

## Phase 1 — Triage (0–15 min)

```
□ Identify source host and user account involved
□ Determine destination: internal, cloud service, external IP, C2?
□ Estimate data volume transferred
□ Classify data type: PII? PHI? IP? Financial? Customer data?
□ Check if transfer is still in progress
□ Confirm this is not an authorised backup or business transfer
□ Escalate to P1 immediately if PII/sensitive data confirmed
□ Notify CISO and Legal within 15 minutes of P1 declaration
```

---

## Phase 2 — Containment (15–60 min)

### Stop the Transfer
```
□ Block destination IP/domain at firewall or proxy immediately
□ Isolate source host from network (coordinate with IT)
□ Suspend the user account if insider threat suspected
□ Revoke cloud service tokens / OAuth grants if cloud exfiltration
□ Block any USB / removable media on the host (EDR policy)
□ Capture memory image of source host before shutdown (forensics)
```

### Preserve Evidence
```
□ Export firewall/proxy logs for the relevant time window NOW
□ Snapshot DLP alerts and reports
□ Export email gateway logs
□ Capture SIEM raw events — do not let them roll off retention
□ Hash and preserve all evidence files
□ Document chain of custody for any physical evidence
```

---

## Phase 3 — Investigation

### Data Scope Assessment
| Question | Finding |
|----------|---------|
| What data was exfiltrated? | |
| How many records? | |
| What classification level? | |
| Does it contain PII (names, emails, DOB, NI numbers)? | |
| Does it contain PHI (medical records)? | |
| Does it contain payment card data? | |
| Which individuals are affected? | |
| What is the estimated business impact? | |

### Attack Path Reconstruction
```
□ Map full kill chain from initial access to exfiltration
□ Identify all hosts accessed by the threat actor
□ Identify all accounts used or compromised
□ Confirm whether data was also encrypted (ransomware double extortion)
□ Determine if exfiltration destination is known threat actor infrastructure
□ Estimate total dwell time (first access to exfiltration)
```

### Insider vs External Threat
| Indicator | External Threat | Insider Threat |
|-----------|----------------|----------------|
| Source | Remote / unusual geolocation | Local network / VPN |
| Timing | Outside business hours | During business hours |
| Access pattern | Unusual for the account | Unusual volume for the user |
| Destination | Threat actor IP / C2 | Personal cloud / email |
| Prior behaviour | No previous access to data | Possibly legitimate access history |

---

## Phase 4 — Legal & Regulatory Notification

> ⚠️ **This section is mandatory if PII, PHI, or customer data was involved.**

### Notification Timeline
| Regulation | Notification Requirement | Deadline |
|------------|-------------------------|----------|
| GDPR (EU/UK) | Notify supervisory authority | 72 hours of discovery |
| PCI-DSS | Notify card brands and acquirer | Immediately |
| HIPAA | Notify HHS and affected individuals | 60 days of discovery |
| NIS2 | Notify national authority | 24 hours (early warning) |
| Local laws | Varies by jurisdiction | Consult Legal |

### Notification Checklist
```
□ Legal counsel notified — Date/Time: ___________
□ CISO notified — Date/Time: ___________
□ DPO (Data Protection Officer) notified — Date/Time: ___________
□ Regulatory body notification submitted — Date/Time: ___________
□ Affected individuals notification prepared — Date/Time: ___________
□ Customer notification (if applicable) — Date/Time: ___________
□ Cyber insurance provider notified — Date/Time: ___________
□ Law enforcement notified (if criminal) — Date/Time: ___________
```

---

## Phase 5 — Eradication & Recovery

```
□ Confirm no additional exfiltration channels remain open
□ Remove threat actor access from all compromised systems
□ Re-image affected hosts
□ Reset all compromised credentials
□ Review and tighten DLP policies
□ Review and tighten cloud access / CASB policies
□ Implement blocking rules for observed exfiltration methods
□ Restore affected systems from clean backups
□ Conduct full vulnerability assessment of affected environment
```

---

## Phase 6 — Lessons Learned

```
□ How did the actor gain initial access?
□ Were DLP controls in place? Were they bypassed?
□ What detection gaps allowed dwell time before discovery?
□ What data classification issues exist?
□ Are CASB / cloud access controls adequate?
□ Were egress controls (firewall, proxy) sufficient?
□ Update this playbook and detection rules based on TTPs observed
```

---

## Escalation Contacts

| Role | Contact | When |
|------|---------|------|
| CISO | ___ | Immediately on P1 |
| Legal Counsel | ___ | Immediately on P1 with data involved |
| DPO | ___ | If PII/PHI confirmed |
| Cyber Insurance | ___ | Within 24 hours |
| Law Enforcement | ___ | If criminal activity confirmed |
| PR / Comms | ___ | If customer notification required |

---

*Related: [Ransomware Response](ransomware-response.md) | [Account Compromise](account-compromise.md) | [Evidence Collection](../runbooks/evidence-collection.md)*
