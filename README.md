# Dovecot XAPS documentation

[![Docs build](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/docs.yml/badge.svg?branch=main)](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/docs.yml)
[![Lint](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/lint.yml/badge.svg?branch=main)](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/lint.yml)
[![External links](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/links.yml/badge.svg?branch=main)](https://github.com/timlaing/dovecot-xaps-docs/actions/workflows/links.yml)
[![Documentation](https://readthedocs.org/projects/dovecot-xaps/badge/?version=latest)](https://dovecot-xaps.readthedocs.io/en/latest/)
[![Dependabot](https://img.shields.io/badge/dependabot-enabled-025e8c)](https://github.com/timlaing/dovecot-xaps-docs/blob/main/.github/dependabot.yml)
[![License](https://img.shields.io/github/license/timlaing/dovecot-xaps-docs)](LICENSE)

Combined administrator documentation for Apple push email with Dovecot. The guide covers installation,
configuration, upgrades, troubleshooting, and the relationship between the plugin, daemon, and signed APT repository.

**[Read the documentation](https://dovecot-xaps.readthedocs.io/)** ·
[Installation guide](https://dovecot-xaps.readthedocs.io/en/latest/getting-started/) ·
[Troubleshooting](https://dovecot-xaps.readthedocs.io/en/latest/troubleshooting/)

## Related repositories

| Repository                                                             | Purpose                                                   |
| ---------------------------------------------------------------------- | --------------------------------------------------------- |
| [dovecot-xaps-plugin](https://github.com/timlaing/dovecot-xaps-plugin) | Dovecot IMAP extension and mail-event forwarding          |
| [dovecot-xaps-daemon](https://github.com/timlaing/dovecot-xaps-daemon) | xapsd registration storage and APNS notification delivery |
| [dovecot-xaps-apt](https://github.com/timlaing/dovecot-xaps-apt)       | Signed Debian packages and APT publishing                 |

Both the plugin and daemon are required for push delivery. Their repositories own the implementation,
configuration templates, package metadata, and build instructions; this repository owns the combined administrator guide.

## Local preview

Use Python 3.14 and Node.js 24:

```sh
git clone https://github.com/timlaing/dovecot-xaps-docs.git
cd dovecot-xaps-docs
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements-dev.txt
npm ci
mkdocs serve
```

Before submitting changes, run:

```sh
npm run lint
mkdocs build --strict
```

Read the Docs publishes the maintained guide from `main` and creates a preview for each documentation PR.
GitHub Actions checks the build, formatting, Markdown, and spelling. External links are checked weekly
and can also be checked manually.

## Contributing and security

See [CONTRIBUTING.md](CONTRIBUTING.md) for setup, validation, and PR guidance, and
[SECURITY.md](SECURITY.md) for private vulnerability reporting. The
[publishing and maintenance guide](docs/contributing.md) explains Read the Docs integration and dependency updates.

## Attribution and license

This guide adapts documentation and configuration examples from the maintained Dovecot XAPS repositories,
which build on the work of Frederik Schwan, Stefan Arentz, and their contributors.
See [attribution](docs/attribution.md) and the [MIT license](LICENSE).
