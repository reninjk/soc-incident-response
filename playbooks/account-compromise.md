# Playbook: Account Compromise / Identity Takeover

**Severity:** P2 (High) — escalate to P1 if admin/privileged account
**Last Reviewed:** 2026-01-01
**Owner:** SOC Manager
**MITRE ATT&CK:** T1078 (Valid Accounts), T1110 (Brute Force), T1556 (Modify Authentication)

---

## Overview

This playbook covers the detection, containment, and recovery from a compromised user or service account — including business email compromise (BEC), credential stuffing, and stolen session token attacks.

---

## Detection Sources

| Source | Indicator |
|--------|-----------|
| SIEM | Multiple failed logins followed by success from new geolocation |
| SIEM | Login from anonymising proxy / Tor exit node |
| SIEM | Impossible travel (two logins from distant locations within minutes) |
| Email Gateway | Forwarding rule created to external address |
| EDR | New device enrolled without MFA prompt |
| User Report | User reports not recognising recent account activity |
| Threat Intel | Credential found in breach dataset (HaveIBeenPwned, dark web) |

---

## Severity Escalation

| Condition | Severity |
|-----------|----------|
| Standard user account | P2 |
| IT admin / privileged account | P1 |
| C-suite / executive account | P1 |
| Service account with API access | P1 |
| Evidence of data exfiltration | P1 |
| Multiple accounts compromised | P1 |

---

## Phase 1 — Triage (0–15 min)

### Initial Assessment
```
□ Identify which account(s) are affected
□ Determine account privilege level
□ Check login history: source IPs, geolocations, user agents
□ Check if MFA was bypassed or disabled
□ Look for forwarding rules, permission changes, or data access
□ Check for lateral movement from the account
□ Identify if credentials are in any known breach datasets
□ Determine how credentials were likely obtained
```

### Key Questions
- When did the compromise likely begin?
- What data/systems did the actor access?
- Is the actor still active in the account?
- Are there other accounts at risk?

---

## Phase 2 — Containment (15–60 min)

### Immediate Actions
```
□ Force sign-out of all active sessions (revoke all tokens)
□ Disable/lock the account temporarily
□ Reset the password (IT team or IAM system)
□ Disable any suspicious email forwarding rules created
□ Remove any suspicious OAuth app grants or delegated access
□ Block the source IP(s) at firewall/proxy if confirmed malicious
□ Alert the affected user via out-of-band channel (phone/in-person)
□ If admin account: audit all actions taken in the compromise window
```

### For Service Accounts
```
□ Rotate all API keys and secrets immediately
□ Audit all systems/services using the account
□ Check for new IAM users or permissions granted
□ Review CloudTrail / audit logs for the account
□ Notify application owners of the service account
```

---

## Phase 3 — Investigation (1–4 hrs)

### Evidence Collection
| Evidence Item | Source | Collected? |
|---------------|--------|------------|
| Login audit logs (full history) | IdP / Active Directory | |
| Email rules and settings snapshot | Exchange / Google Workspace | |
| Files accessed / downloaded | DLP / SharePoint audit | |
| OAuth grants and app permissions | IdP | |
| Browser/device fingerprints | IdP sign-in logs | |
| Source IP WHOIS and threat intel | VirusTotal / AbuseIPDB | |
| Network flows from source IP | SIEM / Firewall | |

### Root Cause Analysis
```
□ Was MFA enabled? If so, how was it bypassed?
□ Was credential obtained via:
   - Phishing / spear-phishing?
   - Credential stuffing (reused password)?
   - Malware / keylogger on endpoint?
   - Insider threat?
   - Third-party breach?
□ Was account subject to brute force / password spray?
□ Were there prior warning indicators that were missed?
```

---

## Phase 4 — Eradication

```
□ Confirm all active sessions terminated
□ Confirm all suspicious OAuth apps revoked
□ Confirm all forwarding rules removed
□ Confirm all suspicious devices removed from account
□ Reset backup codes and trusted devices
□ Re-enrol MFA on a clean device
□ Scan originating endpoint for malware (if internal device)
□ Update any shared passwords the compromised account had access to
□ Notify HR/Legal if insider threat is suspected
```

---

## Phase 5 — Recovery

```
□ Re-enable account with new credentials
□ Enforce strong password + MFA before first login
□ Brief the user on what happened and phishing/credential hygiene
□ Monitor account for 7 days post-recovery for suspicious activity
□ Check for any delayed malicious rules (e.g., auto-delete rules)
□ Review and update conditional access policies if applicable
```

---

## Phase 6 — Lessons Learned

```
□ Was MFA enforced? If not, why not? Remediate immediately.
□ Was impossible travel alerting in place? If not, add detection rule.
□ How was the credential obtained? Close the vector.
□ Was the user previously trained on phishing? Schedule refresher.
□ Update this playbook if new TTPs observed.
```

---

## IOC Collection Template

| IOC Type | Value | Source | First Seen | Notes |
|----------|-------|--------|------------|-------|
| Source IP | | Login logs | | |
| User-Agent | | Login logs | | |
| Email forwarding address | | Exchange audit | | |
| Geolocation | | IdP | | |

---

## Notifications Required

| Recipient | When | Method |
|-----------|------|--------|
| Affected user | Immediately | Phone / in-person |
| User's manager | Within 1 hour | Email |
| IT/IAM team | Immediately | Ticket + phone |
| CISO | If P1 / executive account | Direct call |
| Legal/HR | If insider threat suspected | Secure email |
| Regulatory body | If PII accessed (GDPR 72hr rule) | Per legal guidance |

---

*Related: [Phishing Response](phishing-response.md) | [Data Exfiltration](data-exfiltration.md) | [Alert Triage Runbook](../runbooks/alert-triage.md)*
