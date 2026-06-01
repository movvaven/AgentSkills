# Security Vulnerability Audit (Cursor Agent Skill)

An Agent Skill that runs structured application security reviews against the **OWASP Top 10:2025** checklist. The agent maps findings to severity levels, ties each issue to code locations, and writes a dated audit report you can share with your team.

## Features

- **OWASP Top 10:2025 coverage** — Systematic pass across all ten categories (A01–A10)
- **Per-category reference guides** — Detection hints, prevention guidance, and CWE mappings under `references/`
- **Evidence-based findings** — File paths, line ranges, and config keys (not generic boilerplate)
- **Standardized reports** — Markdown report from `report-template.md`, saved under `audit/YYYY-MM-DD/`
- **Works on local or remote code** — Audits the open workspace, or clones a GitHub repo when the workspace is empty
- **Stack-aware review** — Adapts searches to your languages, frameworks, and deployment config

## Requirements

- An AI-powered IDE with Agent Skills enabled
- For auditing a remote repository from an empty workspace: [GitHub CLI](https://cli.github.com/) (`gh`) or `git`

## Installation

### Option A — Project skill (recommended for a specific repo)

Copy this skill into your application repository:

```bash
git clone https://github.com/movvaven/security-vulnerability-audit.git
mkdir -p .cursor/skills
cp -r security-vulnerability-audit/.cursor/skills/security-vulnerability-audit .cursor/skills/
```

### Option B — User skill (available in all Cursor projects)

```bash
git clone https://github.com/movvaven/security-vulnerability-audit.git
mkdir -p ~/.cursor/skills
cp -r security-vulnerability-audit/.cursor/skills/security-vulnerability-audit ~/.cursor/skills/
```

On Windows (PowerShell), use `$env:movvaven\.cursor\skills` instead of `~/.cursor/skills`.

After installation, open your project in Cursor. The agent loads skills automatically when your request matches the skill description.

## Usage

Ask Cursor Agent for a security review in natural language. Examples:

- “Run a security audit on this codebase.”
- “Review this app for OWASP Top 10 vulnerabilities.”
- “Find security issues and suggest fixes before we ship.”
- “Perform a penetration-style code review on the API layer.”

### What the agent does

1. **Reconnaissance** — Identifies stack, entry points, auth, data stores, dependencies, and CI/CD.
2. **OWASP pass** — Reviews each category using the linked reference files.
3. **Reporting** — Writes `audit/YYYY-MM-DD/security-audit-report.md` (today’s date in ISO format).

If the workspace has no application code, the agent will ask for a GitHub repository URL and clone it (via `gh repo clone` when available).

### Sample output

Reports follow the included template: executive summary, methodology, findings by severity (Critical → Informational), and per-category notes when no issues are found. Optional machine-readable export: `audit/YYYY-MM-DD/findings.json`.

## Repository structure

```
.cursor/skills/security-vulnerability-audit/
├── SKILL.md                 # Skill instructions for the agent
├── report-template.md       # Audit report format
└── references/
    ├── owasp-top10-2025-index.md
    ├── A01-broken-access-control.md
    ├── A02-security-misconfiguration.md
    ├── A03-software-supply-chain-failures.md
    ├── A04-cryptographic-failures.md
    ├── A05-injection.md
    ├── A06-insecure-design.md
    ├── A07-authentication-failures.md
    ├── A08-software-or-data-integrity-failures.md
    ├── A09-security-logging-and-alerting-failures.md
    └── A10-mishandling-of-exceptional-conditions.md
```

Audit artifacts are written **into the project being reviewed** (not into this skill repo):

```
your-app/
└── audit/
    └── 2026-06-01/
        ├── security-audit-report.md
        └── findings.json          # optional
```

## OWASP Top 10:2025 categories

| # | Category |
|---|----------|
| A01 | Broken Access Control |
| A02 | Security Misconfiguration |
| A03 | Software Supply Chain Failures |
| A04 | Cryptographic Failures |
| A05 | Injection |
| A06 | Insecure Design |
| A07 | Authentication Failures |
| A08 | Software or Data Integrity Failures |
| A09 | Security Logging and Alerting Failures |
| A10 | Mishandling of Exceptional Conditions |

Official overview: [OWASP Top 10:2025](https://owasp.org/Top10/2025/)

## Severity scale

| Level | Guidance |
|-------|----------|
| **Critical** | RCE, full data breach, or admin auth bypass with low attacker effort |
| **High** | Serious impact or easy exploitation without strong mitigations |
| **Medium** | Meaningful risk under specific conditions |
| **Low** | Defense-in-depth gaps or limited impact |
| **Informational** | Hardening and best-practice improvements |

## Limitations

This skill guides **automated code and configuration review**. It complements but does not replace:

- Professional penetration testing or red-team exercises
- Runtime DAST against deployed environments (unless you run those tools separately)
- Compliance attestations (SOC 2, PCI, etc.)

Always validate fixes and re-test before production deployment. Reports redact secrets; never commit real credentials into audit output.

## Customization

- Edit `SKILL.md` to change scope, severity rules, or output paths.
- Extend `references/*.md` with organization-specific patterns or internal CWE mappings.
- Adjust `report-template.md` for your team’s reporting standard.

## Contributing

Issues and pull requests are welcome. When adding detection guidance, prefer concrete patterns and CWE links aligned with the [OWASP Top 10:2025](https://owasp.org/Top10/2025/) documentation.

## License

MIT License — see [LICENSE](LICENSE) if present, or add a `LICENSE` file before publishing.

## Acknowledgments

- [OWASP Foundation](https://owasp.org/) — Top 10:2025
