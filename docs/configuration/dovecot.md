# Dovecot configuration

The package installs `/etc/dovecot/conf.d/95-xaps.conf`. Review that file before adding settings
elsewhere: duplicate driver definitions or incorrect plugin order can prevent startup.
Preserve existing mail plugins and administrator changes.

## Dovecot 2.4

The package uses named `mail_plugins` settings and a named push-notification driver.
The following is the package's configuration template:

```dovecot
protocol imap {
  mail_plugins {
    notify = yes
    push_notification = yes
    xaps_push_notification = yes
    xaps_imap = yes
  }
}

protocol lda {
  mail_plugins {
    notify = yes
    push_notification = yes
    xaps_push_notification = yes
  }
}

protocol lmtp {
  mail_plugins {
    notify = yes
    push_notification = yes
    xaps_push_notification = yes
  }
}

# xaps plugin settings (Dovecot 2.4+)
#
# xaps_url:         protocol, hostname and port under which xapsd listens.
#                   xapsd listens on the IPv6 loopback address by default.
# xaps_user_lookup: optional - userdb field to use as the username for push
#                   notifications. Leave empty to use the login username.
#                   For example, if your userdb adds a "masteruser" field,
#                   set this to "masteruser".
#
# HTTP client settings (timeout, retry count, SSL) are configured via
# Dovecot's standard http_client_* settings.
#
xaps_url = http://[::1]:11619
# xaps_user_lookup = theattribute

push_notification xaps {
    driver = xaps
}
```

The three installed modules include a settings module under
`/usr/lib/dovecot/modules/settings/`; mail modules are under `/usr/lib/dovecot/modules/`.
Use a package built for your Dovecot series and architecture.

## Dovecot 2.3

Dovecot 2.3 uses settings expansion and a `plugin` block. Do not paste this syntax into a 2.4 configuration.

```dovecot
protocol imap {
  mail_plugins = $mail_plugins notify push_notification xaps_push_notification xaps_imap
}

protocol lda {
  mail_plugins = $mail_plugins notify push_notification xaps_push_notification
}

protocol lmtp {
  mail_plugins = $mail_plugins notify push_notification xaps_push_notification
}

# xaps plugin settings (Dovecot 2.3)
#
# xaps_url:         protocol, hostname and port under which xapsd listens.
#                   xapsd listens on the IPv6 loopback address by default.
# xaps_user_lookup: optional - userdb field to use as the username for push
#                   notifications. Leave empty to use the login username.
#                   For example, if your userdb adds a "masteruser" field,
#                   set this to "masteruser".
# HTTP client settings (timeout, retry count, SSL) are configured via
# Dovecot's standard http_client_* settings.
plugin {
  xaps_url = http://[::1]:11619
  # xaps_user_lookup = theattribute

  push_notification_driver = xaps:
}
```

Keep `$mail_plugins` expansion intact so existing plugins are retained.
The 2.3 package does not install the 2.4 settings module.

## Validate changes

`xaps_user_lookup` optionally selects a userdb field for the notification username; leave it
unset unless your userdb provides the intended field. HTTP client timeout, retry, and TLS settings
use Dovecot's standard HTTP client settings.

```sh
sudo doveconf -n
sudo systemctl restart dovecot
```

Do not restart if validation fails. Review configuration output locally; redact private values
before sharing it. The maintained templates are in the
[plugin repository](https://github.com/timlaing/dovecot-xaps-plugin).
