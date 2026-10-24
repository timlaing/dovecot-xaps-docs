# Contributing and publishing

Keep the combined administrator guide here. Component repositories own executable configuration,
package scripts, build/test instructions, and their release notes. Link to those sources rather than
copying instructions unnecessarily. When updating copied Dovecot templates, compare them with both
maintained source templates. Update the compatibility baseline when checking new component releases.

For a behavior or packaging change in a component PR, state whether documentation changes are required
and link a companion documentation PR when needed. Preview builds apply to PRs in this documentation
repository, not automatically to PRs in the three component repositories.

## Validate locally

Install `requirements.txt` in a virtual environment, then run:

```sh
mkdocs build --strict
mkdocs serve
```

Check navigation, search, and long command wrapping at phone, tablet, and desktop widths.
Use synthetic examples. Never commit credentials, registration data, or private server information.
The GitHub Docs check runs a strict build; Read the Docs provides the hosted PR preview.

## Read the Docs setup

1. Connect `https://github.com/timlaing/dovecot-xaps-docs` to the registered `dovecot-xaps` project
   through the Read the Docs GitHub App. Give the app access to this repository.
2. Set the default branch and documentation version to `main` and `latest` respectively.
3. Use the root `.readthedocs.yaml` configuration and trigger the first build.
4. Enable **Settings → Pull request builds → Build pull requests for this project**.
5. Enable preview notifications under **Settings → Addons → Notifications** so previews are identifiable.
6. Open a documentation PR and verify its Read the Docs preview URL and status check.
7. After that check has appeared, require it and the GitHub **Docs / Build** check in the branch rules.

Read the Docs owns live publishing and preview URLs. GitHub Actions validates PRs and `main` without
publishing to Pages. No Read the Docs token is needed in the repository for GitHub App integration.
Do not add a global site redirect to a version-specific preview host.

See the official [MkDocs integration](https://docs.readthedocs.com/platform/stable/intro/mkdocs.html)
and [PR preview setup](https://docs.readthedocs.com/platform/stable/guides/pull-requests.html).

## Review a documentation preview

Open the Read the Docs check on the PR and follow its preview link. Confirm that the preview
contains the proposed text, internal links resolve within that preview, and the site search
finds an affected topic. Use the preview notification to distinguish the PR from the published guide.
After merging, check that the same change appears in `latest`.

## Maintenance checks

Install Node.js 24 and run `npm ci`, then `npm run lint`. This checks formatting, Markdown,
and spelling. Use `npm run format` to format files. Add project-specific dictionary words in
lowercase and alphabetical order. Internal links are checked by the strict MkDocs build;
the separate External links workflow runs weekly and can be started manually.

Install `requirements-dev.txt` to obtain the pinned dependency compiler. Update the direct
requirements in `requirements.in`, then regenerate the lock file with Python 3.14:

```sh
python -m pip install -r requirements-dev.txt
pip-compile --upgrade --strip-extras --output-file requirements.txt requirements.in
python -m pip install -r requirements.txt
mkdocs build --strict
```

Commit the input and generated requirements together. Dependabot checks Python, npm, and
GitHub Actions dependencies weekly. Review its compiled requirements updates and regenerate
the lock file when needed; do not merge dependency PRs with failing builds or previews.
The lock file pins runtime dependencies; the compiler is a development-only dependency.

The main branch requires PRs, resolved review conversations, the Build and Lint checks, and
the Read the Docs preview check. CODEOWNERS routes reviews to @timlaing. The wiki is disabled
so maintained documentation stays in this repository.
