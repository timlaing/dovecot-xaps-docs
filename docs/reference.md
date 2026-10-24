# Reference and compatibility

This initial guide was prepared from the component source and local package indexes on 24 October 2026.
Those indexes contain plugin `1.1.1` and daemon `1.4.1`, for both `amd64` and `arm64`.
These are the documented baseline, not an automatically updated statement of the latest release.

Plugin and daemon versions are independent. The documentation follows `main`; use component release notes
and package candidates when planning upgrades.

| Path / address                       | Purpose                           |
| ------------------------------------ | --------------------------------- |
| `http://[::1]:11619`                 | Default plugin-to-daemon endpoint |
| `/etc/dovecot/conf.d/95-xaps.conf`   | Packaged Dovecot configuration    |
| `/usr/lib/dovecot/modules/`          | Dovecot mail modules              |
| `/usr/lib/dovecot/modules/settings/` | Dovecot 2.4 settings module       |
| `/etc/xapsd/xapsd.yaml`              | Daemon configuration              |
| `/etc/xapsd/AuthKey_<KeyID>.p8`      | APNS private key                  |
| `/var/lib/xapsd/database.json`       | Default registration database     |

## Source references

- [Plugin configuration and build instructions](https://github.com/timlaing/dovecot-xaps-plugin#readme)
- [Daemon configuration and build instructions](https://github.com/timlaing/dovecot-xaps-daemon#readme)
- [APT distribution definitions](https://github.com/timlaing/dovecot-xaps-apt/blob/main/conf/distributions)
- [Plugin release notes](https://github.com/timlaing/dovecot-xaps-plugin/releases)
- [Daemon release notes](https://github.com/timlaing/dovecot-xaps-daemon/releases)

## Verification scope

The package targets are Ubuntu 26.04 with Dovecot 2.4.2 or later in the 2.4 series, and Ubuntu 24.04
with Dovecot 2.3. A documentation build checks rendering and internal links. It does not install
packages, establish ABI compatibility, validate administrator configuration, or verify Apple delivery.
Record installation and real-device checks separately when updating this guide.

## Installation verification on 24 October 2026

| Environment                         | Packages                              | Result |
| ----------------------------------- | ------------------------------------- | ------ |
| Ubuntu 26.04, arm64, Dovecot 2.4.2  | Plugin `1.1.1-1`, xapsd `1.4.1`       | Pass   |
| Ubuntu 24.04, arm64, Dovecot 2.3.21 | Plugin `1.1.1-1~dov23`, xapsd `1.4.1` | Pass   |

Each check used a fresh disposable official Ubuntu container, verified the repository key fingerprint,
updated APT through the signed repository, installed both packages, and ran `doveconf -n` successfully.
The noninteractive installer was supplied clearly synthetic APNS topic, key ID, and team ID values
through debconf. Empty noninteractive answers are rejected by the daemon installer; interactive users
must provide their own valid values when prompted.

These checks did not start a complete systemd mail server, exercise upgrades, test amd64 packages,
or send APNS notifications. Service startup and physical iOS delivery remain not run. A physical-device
test requires a configured mail server, suitable credentials, and an iOS Mail account.
