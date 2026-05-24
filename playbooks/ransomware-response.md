# Ransomware Incident Response Playbook

**Severity:** P1 - Critical (always)
**MITRE ATT&CK:** T1486 - Data Encrypted for Impact, T1490 - Inhibit System Recovery
**Last Updated:** 2026-05

---

## ⚠️ IMMEDIATE ACTIONS (First 15 Minutes)

1. **DO NOT reboot** affected systems — may destroy forensic evidence
2. Isolate affected hosts from network IMMEDIATELY (pull cable / disable NIC)
3. Notify SOC Manager and CISO
4. Preserve a memory dump before any remediation
5. Activate the Incident Response Team

---

## 1. Detection Sources
- EDR: mass file encryption events, shadow copy deletion
- SIEM: rapid file rename events (.locked, .encrypted extensions)
- Backup system alerts
- User reports: files inaccessible, ransom note on desktop

## 2. Initial Triage
- [ ] Identify patient zero (first infected host)
- [ ] Determine ransomware family (check ransom note, file extension, ID Ransomware)
- [ ] Scope the blast radius: how many hosts/shares affected?
- [ ] Check backup integrity — are backups encrypted too?
- [ ] Identify the initial access vector (phishing, RDP brute force, vulnerability)

## 3. Containment

### Network Isolation
1. Isolate all confirmed and suspected hosts (VLAN segregation or full disconnect)
2. Block C2 IPs/domains at firewall immediately
3. Disable compromised accounts
4. Revoke VPN access for affected users
5. Block lateral movement — disable SMB/RDP where possible

### Preserve Evidence
1. Take memory dumps of affected systems
2. Preserve event logs, EDR telemetry
3. Document the ransom note and file extensions
4. Capture network traffic logs

## 4. Identify Ransomware Family
- Upload sample to ID Ransomware (https://id-ransomware.malwarehunterteam.com/)
- Check if decryptor exists (No More Ransom: https://www.nomoreransom.org/)
- Review threat intel feeds for known indicators

## 5. Eradication
1. Rebuild affected systems from clean images
2. Remove ransomware artifacts from all hosts
3. Reset ALL credentials (assume full compromise)
4. Patch the initial access vector immediately
5. Block all identified IOCs enterprise-wide

## 6. Recovery
1. Restore from last known-good backups (validate integrity first)
2. Bring systems back online in a controlled, staged manner
3. Monitor closely for re-infection during recovery
4. Verify backup restore completeness before resuming operations

## 7. Notification & Legal
- [ ] Notify Legal/Compliance immediately if PII involved
- [ ] Check regulatory reporting requirements (GDPR 72h, HIPAA, etc.)
- [ ] Document decision on ransom payment (consult Legal; generally do NOT pay)
- [ ] Engage cyber insurance carrier
- [ ] Coordinate with Law Enforcement if required

## 8. Post-Incident Actions
- [ ] Complete full forensic investigation to determine scope
- [ ] Full post-incident review within 72 hours
- [ ] Update detection rules for this ransomware family
- [ ] Review and harden RDP, email, and patch management
- [ ] Test and validate backup/restore procedures

## 9. Key Contacts
| Contact | Role | Trigger |
|---------|------|---------|
| SOC Manager | Incident lead | Immediate |
| CISO | Executive escalation | Immediate |
| Legal/Compliance | PII/regulatory | Immediate |
| IT Infrastructure | Isolation support | Immediate |
| Cyber Insurance | Claim filing | Within 24h |

---
*See: [Incident Report Template](../templates/incident-report-template.md)*
