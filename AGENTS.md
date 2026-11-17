# Repository guidance

This repository owns the combined Dovecot XAPS administrator guide. Preserve upstream attribution.
Use `mkdocs build --strict` for documentation validation. Report installation and real-device checks
separately; a docs build does not prove Dovecot or APNS compatibility.
Keep APNS credentials, device tokens, account/email data, and private server details out of all files.
Read the Docs publishes the site and PR previews. GitHub Actions only validates documentation.
Reference GitHub Actions by full commit SHAs, with version comments where available.
Do not commit, push, or change repository/service settings without user authorization.
