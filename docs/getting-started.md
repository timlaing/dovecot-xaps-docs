# Getting started

Start with a working Dovecot server, an iOS Mail account, mail delivery through LDA or LMTP,
and suitable APNS credentials. Back up configuration before installing packages.

## Choose the matching packages

| Dovecot series       | Example operating system | APT suite  | Architectures    |
| -------------------- | ------------------------ | ---------- | ---------------- |
| 2.4 (2.4.2 or later) | Ubuntu 26.04             | `resolute` | `amd64`, `arm64` |
| 2.3                  | Ubuntu 24.04             | `noble`    | `amd64`, `arm64` |

Check `dovecot --version` and `dpkg --print-architecture` first. These are the package targets;
they are not a claim that every Debian or Ubuntu release is supported. Do not mix suites.

## Add the signed APT repository

Download the public key:

```sh
curl -fsSL https://raw.githubusercontent.com/timlaing/dovecot-xaps-apt/main/public-key.asc -o /tmp/dovecot-xaps-public-key.asc
gpg --show-keys --with-fingerprint /tmp/dovecot-xaps-public-key.asc
```

Verify the full fingerprint against the repository's
[signing-key documentation](https://github.com/timlaing/dovecot-xaps-apt#signing-key)
and an independently trusted copy before trusting the key. The fingerprint recorded when this
guide was prepared is `D6F4 7642 A542 4C78 332C CD8E EAD2 766F 4E85 7693`.

```sh
sudo gpg --dearmor --yes -o /usr/share/keyrings/dovecot-xaps-archive-keyring.gpg /tmp/dovecot-xaps-public-key.asc
sudo chmod 0644 /usr/share/keyrings/dovecot-xaps-archive-keyring.gpg
```

For Dovecot 2.4, add `resolute`:

```sh
echo "deb [signed-by=/usr/share/keyrings/dovecot-xaps-archive-keyring.gpg] https://timlaing.github.io/dovecot-xaps-apt resolute main" | sudo tee /etc/apt/sources.list.d/dovecot-xaps.list
```

For Dovecot 2.3, use this line **instead**:

```sh
echo "deb [signed-by=/usr/share/keyrings/dovecot-xaps-archive-keyring.gpg] https://timlaing.github.io/dovecot-xaps-apt noble main" | sudo tee /etc/apt/sources.list.d/dovecot-xaps.list
```

Install both components:

```sh
sudo apt-get update
apt-cache policy dovecot-xaps-plugin xapsd
sudo apt-get install dovecot-xaps-plugin xapsd
```

APT verifies repository signatures. Automatic upgrades depend on your local update policy;
adding this repository alone does not configure unattended upgrades.

## Configure and start

Install interactively so the daemon installer can prompt for the APNS topic, key ID, and team ID.
For unattended installation, preseed the `xapsd/key-file-topic`, `xapsd/key-file-key-id`, and
`xapsd/key-file-team-id` debconf answers; empty values are rejected. The installer derives
`AuthKey_<KeyID>.p8`. Copy your private key into place, replacing `<KeyID>` with the actual ID:

```sh
sudo install -o root -g xapsd -m 0640 AuthKey_<KeyID>.p8 /etc/xapsd/AuthKey_<KeyID>.p8
```

Review [daemon configuration](configuration/daemon.md) and the installed
[Dovecot configuration](configuration/dovecot.md). Both must use the same listener:
`http://[::1]:11619`.

```sh
sudo systemctl enable --now xapsd
sudo systemctl restart xapsd
sudo doveconf -n
sudo systemctl restart dovecot
sudo systemctl status dovecot xapsd
```

Restart Dovecot only after `doveconf -n` succeeds. The daemon package may start the service before
you have installed credentials; restart it after configuration is complete.

## Register and verify iOS Mail

Reconnect the Mail account, for example by toggling Airplane Mode. Inspect the service logs for
successful registration, then deliver a test message through LDA or LMTP and verify that the
physical device receives a notification. An HTTP `200 OK` registration alone does not prove APNS delivery.
See [troubleshooting](troubleshooting.md) if either step fails.

## Other installation methods

Download matching packages from the [plugin releases](https://github.com/timlaing/dovecot-xaps-plugin/releases)
and [daemon releases](https://github.com/timlaing/dovecot-xaps-daemon/releases), then install with
`sudo apt install ./package.deb`. Source-build instructions remain in the component READMEs.
