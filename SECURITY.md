# Security Policy

This policy covers all repositories under the
[aiperceivable](https://github.com/aiperceivable) organization. A repository with its own
`SECURITY.md` overrides this file.

## Reporting a Vulnerability

**Do NOT open a public issue for a security vulnerability.**

Use GitHub's private vulnerability reporting:

1. Go to the affected repository's **Security** tab
2. Click **Report a vulnerability**
3. Fill in the details

Alternatively, email **team@aiperceivable.org**.

A report is more actionable when it includes the affected repository and version, the
conditions required to trigger the issue, and what an attacker gains. A proof of concept
helps but is not required to file.

### Governance boundaries are in scope

apcore exists to enforce validation, ACL, approval gates, and audit at the execution
boundary. Treat any way of **reaching application logic while bypassing those controls**
as a security issue rather than a functional bug — for example:

- An ACL or approval gate that a specific surface (MCP, A2A, CLI, HTTP) fails to apply
- A denial that leaks information the caller was not entitled to
- A call chain that escalates the effective identity of a request
- An audit path that can be made to omit an execution that actually happened

These are exactly the failures the project is meant to prevent, so they are worth reporting
even when the impact looks narrow.

## Response Timeline

| Stage | Target |
|---|---|
| Acknowledgment | Within 48 hours |
| Initial assessment | Within 7 days |
| Fix or mitigation | Depends on severity; critical issues targeted within 30 days |

## Supported Versions

The latest published release of each component receives security fixes. Older versions are
handled on a best-effort basis.

## Disclosure Policy

We follow coordinated disclosure:

1. Reporter submits the vulnerability privately
2. We confirm and assess the issue
3. We develop and test a fix
4. We release the fix and publish an advisory
5. Reporter is credited, unless they prefer to remain anonymous
