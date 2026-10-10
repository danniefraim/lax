# Security policy

Lax handles very sensitive data, so we take security reports seriously.

## Reporting a vulnerability

**Please do not open a public issue.** Instead, report it privately through
GitHub: go to the **Security** tab of this repository and click
**Report a vulnerability**.

Please include what you found, how to reproduce it, and what impact you think it has.
Don't include real personal data in your report.

You can expect an acknowledgement within a week. Lax is a volunteer project, so
fixes may take time, but we'll keep you updated.

## Scope

Especially interested in:

- Data leaving the machine without the user's consent
- Capturing content that should be excluded (passwords, private windows, excluded apps)
- Prompt injection that changes memory or behaviour in ways the user didn't intend
- Secrets (API keys) exposed outside the macOS Keychain

See [docs/GUARDRAILS.md](docs/GUARDRAILS.md) for the rules Lax is supposed to follow.
