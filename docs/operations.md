# Operations

## Service checks

```sh
sudo systemctl status dovecot xapsd
sudo journalctl -u dovecot -u xapsd -n 100 --no-pager
sudo ss -ltnp | grep 11619
```

Logs can contain private account or device information. Review locally and redact before sharing.

## Upgrade

Back up configuration and registration data, review component release notes, then inspect candidates:

```sh
sudo apt-get update
apt-cache policy dovecot-xaps-plugin xapsd
sudo apt-get install --only-upgrade dovecot-xaps-plugin xapsd
sudo doveconf -n
sudo systemctl status dovecot xapsd
```

The daemon package preserves existing YAML settings; Dovecot configuration is administrator-owned.
Review conffile prompts rather than blindly replacing local settings. The daemon's package-managed
service restarts on upgrade. Plan a maintenance window, validate Dovecot configuration, and restart
Dovecot after the plugin upgrade to load the new modules. Verify registration and delivery afterward.

Changing Dovecot series requires the matching APT suite and plugin package. Do not assume a normal
package upgrade migrates Dovecot configuration syntax.

## Back up and restore

Keep a protected backup of `/etc/xapsd/`, the relevant Dovecot configuration, and
`/var/lib/xapsd/database.json` (or your configured `databaseFile`). Credentials and registration data
are private. Stop xapsd while copying the database to obtain a consistent snapshot, then start it.

To restore, stop xapsd, restore configuration, credentials, and database with their original ownership
and permissions, then start xapsd. Validate Dovecot configuration and verify delivery on a device.
Do not attach database backups to support issues.

## Remove

Remove xapsd and the plugin with APT during a maintenance window. Remove any manually added XAPS
configuration before restarting Dovecot, then run `doveconf -n`. Removal and purge have different
configuration-retention behavior: back up first and inspect package scripts before purging.
Remove the APT source entry and key only when you no longer need the repository.
