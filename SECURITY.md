# Security Policy

## Reporting a Security Vulnerability

If you discover a security vulnerability in this repository or its content, **do not open a public GitHub issue**.

Instead, please report it responsibly:

**Email:** security@[your-organisation].com  
**Subject:** `[SECURITY] soc-incident-response — <brief description>`

We will acknowledge your report within **24 hours** and aim to provide a resolution timeline within **72 hours**.

## Scope

This repository contains **documentation, templates, and scripts** for security operations. Vulnerabilities relevant to this repo include:

| In Scope | Out of Scope |
|----------|-------------|
| Sensitive data accidentally committed (credentials, PII, real IP addresses) | Theoretical vulnerabilities with no practical impact |
| Scripts with security flaws that could cause harm if executed | Issues already publicly disclosed |
| Playbooks recommending actions that could cause damage | Spam or social engineering |
| Exposed internal network information | |

## Sensitive Data Policy

This repository must **never** contain:

- Real credentials, API keys, or passwords
- Real internal IP addresses or hostnames
- Customer data or PII
- Confidential investigation details
- Internal network topology

If you find any of the above committed by accident, report it immediately — we will rotate credentials and purge history.

## Response Process

1. **Acknowledge** — We confirm receipt within 24 hours
2. **Assess** — We evaluate severity and impact within 72 hours
3. **Remediate** — We fix the issue (rotate secrets, redact data, patch scripts)
4. **Disclose** — We credit responsible disclosures in release notes (unless anonymity requested)

## Supported Versions

We actively maintain the `main` branch. Only the latest version is supported.

## Security Best Practices for Contributors

When contributing to this repo:

- Never test scripts against production systems without change approval
- Never commit `.env` files or files containing real secrets
- Use `git-secrets` or similar pre-commit hooks to prevent accidental credential commits
- Review the [CONTRIBUTING.md](CONTRIBUTING.md) sensitive data policy before every commit
