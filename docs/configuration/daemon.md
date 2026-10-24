# Daemon configuration

xapsd reads `/etc/xapsd/xapsd.yaml`. Preserve existing values during upgrades.
Keep private keys under `/etc/xapsd/` with access restricted to root and the `xapsd` service group.

## Settings

| Setting         | Purpose / shipped default                                  |
| --------------- | ---------------------------------------------------------- |
| `loglevel`      | Logging level; `info`                                      |
| `listenAddr`    | Plugin HTTP listener; `'[::1]'`                            |
| `port`          | Plugin HTTP port; `11619`                                  |
| `databaseFile`  | Persistent registrations; `/var/lib/xapsd/database.json`   |
| `keyFileP8`     | Private-key filename, normally `AuthKey_<KeyID>.p8`        |
| `keyFileTopic`  | APNS topic matching your credentials                       |
| `keyFileKeyId`  | Apple key ID                                               |
| `keyFileTeamId` | Apple Developer team ID                                    |
| `checkInterval` | Delayed-event check interval in seconds; `20`              |
| `delay`         | Delay for events other than new messages, in seconds; `30` |

The sample topic `com.apple.mail.nil` is a placeholder, not a guarantee of usable credentials.
Use the topic belonging to your credential. Do not publish your completed configuration.

The [shipped sample](https://github.com/timlaing/dovecot-xaps-daemon/blob/main/configs/xapsd/xapsd.yaml)
also documents legacy certificate options (`certificateFileP12`, `certificateFilePem`,
`certificateFilePemKey`) and optional proxy TLS settings (`tlsCertfile`, `tlsKeyfile`,
`tlsListenAddr`, `tlsPort`, default port `11620`). Direct plugin use of the HTTPS listener is
unsupported; keep the default loopback HTTP connection for the normal same-host deployment.

## Apply changes

```sh
sudo systemctl restart xapsd
sudo systemctl status xapsd
sudo journalctl -u xapsd -n 100 --no-pager
```

Keep the listener aligned with Dovecot's `xaps_url = http://[::1]:11619`.
The registration HTTP service carries private data; do not expose it to an untrusted network.
