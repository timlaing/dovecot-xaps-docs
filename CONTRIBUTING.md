# Contributing to dovecot-xaps-docs

This repository maintains the combined administrator guide for
[dovecot-xaps-plugin](https://github.com/timlaing/dovecot-xaps-plugin),
[dovecot-xaps-daemon](https://github.com/timlaing/dovecot-xaps-daemon), and
[dovecot-xaps-apt](https://github.com/timlaing/dovecot-xaps-apt).
Preserve attribution to the original projects and contributors.

## Before you start

Search the existing issues and PRs. Use the documentation issue forms for missing instructions,
incorrect examples, or broken links. Report implementation bugs in the relevant component repository.
Report vulnerabilities privately using [SECURITY.md](SECURITY.md).

## Development setup

Use Python 3.14 and Node.js 24. Fork the repository, clone your fork, then install the tools:

```sh
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements-dev.txt
npm ci
mkdocs serve
```

The local preview is available at the address printed by MkDocs.

## Making changes

1. Create a focused branch and keep each PR to one self-contained change.
2. Edit the administrator guide under `docs/`. Keep component build instructions and executable
   configuration in their source repositories and link to them.
3. Check examples against the relevant source templates. Keep Dovecot 2.3 and 2.4 syntax separate.
4. Update navigation in `mkdocs.yml` when adding a guide page.
5. Use synthetic examples. Never commit APNS keys, credentials, device tokens, account or email data,
   registration databases, or private server details.
6. Add project-specific spelling words to `cspell.json` in lowercase and alphabetical order.

## Verification

```sh
npm run format
npm run lint
mkdocs build --strict
```

Review formatting changes before committing. Check the Read the Docs PR preview, navigation,
search, command wrapping, and layout on phone, tablet, and desktop widths.
The separate External links workflow runs weekly and can be started manually by a maintainer.

Report checks as pass, fail, or not run. A successful documentation build does not prove package
installation, Dovecot compatibility, service startup, or physical iOS/APNS delivery. Validate changed
installation instructions in disposable environments and state the OS, architecture, and component versions.

## Pull requests

Complete the PR template, link related issues and component PRs, and allow maintainer edits.
Use a concise title with a prefix such as `docs:`, `fix:`, or `chore:`.
The main branch requires a PR, resolved review conversations, and successful Build, Lint, and Read the Docs preview checks.

See the [publishing and maintenance guide](docs/contributing.md) for dependency lock regeneration,
Read the Docs configuration, and the documentation ownership rules.
