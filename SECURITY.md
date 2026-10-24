# Security Policy

## Supported documentation

Security corrections are maintained on `main` and published to the `latest` Read the Docs version.
PR previews and historical commits are development or archival material; use the maintained guide
when configuring a server. Component release support is defined in each component's security policy.

## Reporting a vulnerability

Do not report vulnerabilities in public issues or PRs. Use
[GitHub private vulnerability reporting](https://github.com/timlaing/dovecot-xaps-docs/security/advisories/new)
to contact the maintainers about this documentation repository.

Include the affected page URL and version or commit, the security impact, reproduction steps,
and any suggested correction or mitigation. For dependency or workflow problems, identify the affected
file or dependency version. Use redacted examples; do not send private keys, passwords, device tokens,
account data, registration databases, email data, or private server details.

Maintainers will acknowledge reports as soon as practical and coordinate remediation and disclosure
with the reporter. Credit can be provided if requested.

## Scope and related projects

This policy covers unsafe documentation instructions, accidentally published sensitive data,
the documentation build dependencies, and this repository's publishing and maintenance workflows.

Report implementation and packaging vulnerabilities to the relevant project privately:

- [Dovecot plugin security policy](https://github.com/timlaing/dovecot-xaps-plugin/blob/main/SECURITY.md)
- [xapsd security policy](https://github.com/timlaing/dovecot-xaps-daemon/blob/main/SECURITY.md)
- [APT repository security policy](https://github.com/timlaing/dovecot-xaps-apt/blob/main/SECURITY.md)

Vulnerabilities in Dovecot, Apple services, Read the Docs, or the host operating system should be
reported to their respective maintainers.

## Security checks

Required documentation build, lint, and preview checks protect the main branch. Dependabot checks
Python, npm, and GitHub Actions dependencies weekly. These automated checks complement security review;
they do not establish that a deployed mail server is secure or that APNS delivery works.
