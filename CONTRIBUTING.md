# Contributing to SOC Incident Response

Thank you for contributing to this SOC knowledge base. These guidelines ensure content stays accurate, safe, and consistent.

## Getting Started

1. **Fork** the repository and create a feature branch: `git checkout -b feat/your-change`
2. Make your changes following the standards below
3. Submit a **Pull Request** using the PR template — every PR needs at least one review
4. All CI checks must pass before merging

## Content Standards

### Playbooks
- Follow the structure: Overview → Detection Sources → Severity → Phases (Triage → Contain → Investigate → Eradicate → Recover → Lessons Learned)
- Every playbook must map to at least one **MITRE ATT&CK** tactic/technique
- Use checkbox syntax (`- [ ]`) for all action items so they can be ticked off during incidents
- Link to related runbooks and templates
- Include a **Severity Classification** table

### Runbooks
- Step-by-step, numbered, executable commands where applicable
- Include **Prerequisites** section at the top
- Include **Escalation** section at the bottom
- Shell commands in fenced code blocks with language annotation (```powershell / ```bash)
- Estimated completion time in the header

### Templates
- All templates use fillable fields: `_______________` for text, `[ ]` for checkboxes
- Include classification label (CONFIDENTIAL / INTERNAL)
- Include sign-off section

## Sensitive Data Policy

**Never commit:**
- Real IP addresses of internal systems
- Real usernames, email addresses, or employee names
- API keys, passwords, or credentials of any kind
- Customer data or PII
- Details of active/unresolved incidents
- Internal network topology or asset inventory

Use placeholders like `192.168.x.x`, `user@company.com`, `HOSTNAME`.

## Review Process

| Change Type | Reviews Required | Approval |
|------------|-----------------|---------|
| New playbook / runbook | 2 SOC team members | SOC Manager |
| Update to existing content | 1 SOC team member | SOC Manager |
| Template changes | 1 SOC team member | SOC Manager |
| CI / workflow changes | SOC Manager | SOC Manager |
| Typo / formatting only | 1 reviewer | Auto-merge allowed |

## Commit Message Format

Use conventional commits:
```
feat: add insider threat playbook
fix: correct MTTR target in escalation matrix
docs: update phishing playbook with new email gateway steps
chore: update CI to use actions/checkout@v4
refactor: restructure ransomware playbook phases
```

## Playbook Review Cycle

All production playbooks must be reviewed:
- **P1/P2 playbooks:** every 3 months
- **P3/P4 playbooks:** every 6 months
- **After any real incident** that uses the playbook: immediate review

Tag stale playbooks with `needs-review` label if overdue.

## Questions?

Open an issue or contact the SOC Manager directly.
