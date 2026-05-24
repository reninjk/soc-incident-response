# Phishing Incident Response Playbook

**Severity:** P3 (single user) → P1 (credential harvesting/mass campaign)
**MITRE ATT&CK:** T1566 - Phishing
**Last Updated:** 2026-05

---

## 1. Detection Sources
- Email gateway alerts (suspicious links, attachments)
- User-reported suspicious emails
- SIEM: multiple login failures after clicking link
- EDR: browser spawning unusual child processes

## 2. Initial Triage Checklist
- [ ] Identify the sender and full email headers
- [ ] Check if link/attachment is malicious (VirusTotal, URLScan.io)
- [ ] Determine how many users received the email
- [ ] Check if any user clicked the link or opened the attachment
- [ ] Check if credentials were entered on a phishing page

## 3. Containment Steps

### No user clicked:
1. Block sender domain at email gateway
2. Remove email from all mailboxes (admin purge)
3. Add URL/domain to proxy/DNS blocklist

### User clicked (no credential entry):
1. Block URL/IP at proxy and firewall
2. Trigger EDR full scan on the endpoint
3. Review browser history and DNS/proxy logs
4. Notify user; provide security awareness reminder

### Credentials entered:
1. **IMMEDIATE:** Force password reset for affected account(s)
2. Revoke all active sessions and tokens
3. Enable/enforce MFA if not already active
4. Check for malicious mail forwarding rules or OAuth grants
5. Review login history for anomalous activity (geo-IP, time)
6. Escalate to P2; notify SOC Manager

## 4. Eradication
- Remove malicious email from all mailboxes
- Block all IOCs (URLs, IPs, file hashes, sending domains, reply-to)
- Run enterprise-wide threat hunt for the phishing domain
- Update email filtering rules and DMARC/SPF/DKIM config

## 5. Recovery
- Restore compromised account access after verification
- Confirm no persistence mechanisms (backdoors, new OAuth apps)
- Reimage endpoint if malware was executed

## 6. Post-Incident Actions
- [ ] Complete incident report (see templates/incident-report-template.md)
- [ ] Update IOC blocklist
- [ ] Send phishing awareness comms to affected org
- [ ] Schedule phishing simulation within 30 days
- [ ] Review and tune email gateway rules

## 7. Evidence to Collect
| Artifact | Location |
|----------|----------|
| Full email headers | Email admin console |
| Message-ID | Email metadata |
| URLs/links extracted | Email body |
| Attachment hashes (MD5, SHA256) | EDR / sandbox |
| Screenshot of phishing page | Analyst workstation |
| Proxy logs for phishing domain | SIEM |
| Browser history timestamps | EDR forensics |

---
*See: [Incident Report Template](../templates/incident-report-template.md) | [Alert Triage Runbook](../runbooks/alert-triage.md)*
