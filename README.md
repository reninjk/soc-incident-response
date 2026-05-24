# 🛡️ SOC Incident Response

> Centralized repository of incident response playbooks, runbooks, IR templates, and escalation procedures for the Security Operations Center.

## 📁 Repository Structure

```
soc-incident-response/
├── playbooks/
│   ├── phishing-response.md
│   ├── ransomware-response.md
│   ├── account-compromise.md
│   ├── malware-detection.md
│   └── data-exfiltration.md
├── runbooks/
│   ├── alert-triage.md
│   ├── ioc-enrichment.md
│   ├── host-isolation.md
│   └── evidence-collection.md
├── templates/
│   ├── incident-report-template.md
│   ├── post-incident-review.md
│   └── escalation-matrix.md
├── severity-matrix/
│   └── severity-classification.md
└── .github/
    └── workflows/
        └── validate-markdown.yml
```

## 🚨 Incident Severity Levels

| Severity | Response Time | Examples |
|----------|--------------|---------|
| **P1 - Critical** | 15 min | Ransomware, Active breach, Data exfiltration |
| **P2 - High** | 1 hour | Account compromise, Malware spread |
| **P3 - Medium** | 4 hours | Phishing attempt, Single endpoint malware |
| **P4 - Low** | 24 hours | Policy violation, Suspicious log activity |

## 🔄 Incident Response Lifecycle

1. **Detection** → Alert fires from SIEM/EDR/email gateway
2. **Triage** → Analyst validates and assigns severity
3. **Containment** → Isolate affected systems/accounts
4. **Eradication** → Remove threat artifacts
5. **Recovery** → Restore systems to normal operations
6. **Lessons Learned** → Post-incident review within 72h

## 📞 Escalation Contacts

| Role | Escalation Trigger |
|------|-------------------|
| SOC Manager | P1/P2 incidents, or after 2h unresolved P3 |
| CISO | Active data breach, ransomware deployment |
| Legal/Compliance | PII involved, regulatory notification required |
| IT Leadership | Business-critical systems impacted |

## 🔗 Related Repositories

- [soc-detection-rules](../soc-detection-rules) — SIEM detection rules & Sigma rules
- [soc-automation](../soc-automation) — Response automation scripts
- [soc-compliance-reporting](../soc-compliance-reporting) — Audit & compliance docs

## 🏷️ MITRE ATT&CK Coverage

Playbooks in this repo map to the following MITRE ATT&CK tactics:
- Initial Access (TA0001)
- Execution (TA0002)
- Persistence (TA0003)
- Credential Access (TA0006)
- Exfiltration (TA0010)
- Impact (TA0040)

---
*Maintained by the SOC Manager | Review cycle: Quarterly*
