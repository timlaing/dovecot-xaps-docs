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
