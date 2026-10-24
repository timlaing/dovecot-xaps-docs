# Apple push email for Dovecot

Dovecot XAPS connects a Dovecot mail server to Apple Push Notification Service (APNS)
so iOS Mail can receive push notifications.

[Start the installation guide](getting-started.md).

## How it works

1. iOS Mail registers through Dovecot's `XAPPLEPUSHSERVICE` IMAP extension.
2. The plugin forwards registrations and mail events to xapsd over HTTP.
3. xapsd stores registrations and sends notifications through APNS.
4. iOS Mail reconnects to the mail server to synchronise mail.

Both the plugin and daemon are required. Mail must be delivered through Dovecot LDA or LMTP.
The APT repository distributes the packages; it is not a running part of mail delivery.

## Components

| Component      | Purpose                                  | Source                                                                 |
| -------------- | ---------------------------------------- | ---------------------------------------------------------------------- |
| Plugin         | IMAP extension and mail-event forwarding | [dovecot-xaps-plugin](https://github.com/timlaing/dovecot-xaps-plugin) |
| Daemon         | Registration storage and APNS delivery   | [dovecot-xaps-daemon](https://github.com/timlaing/dovecot-xaps-daemon) |
| APT repository | Signed packages                          | [dovecot-xaps-apt](https://github.com/timlaing/dovecot-xaps-apt)       |

## Requirements and privacy

Use APNS credentials that you are legally entitled to use. Apple has not published an
XAPPLEPUSHSERVICE specification; this implementation derives from Apple's published Dovecot patches.
Obtaining suitable APNS credentials is a prerequisite, and this guide does not promise that an
arbitrary Apple Developer key or topic will work with iOS Mail.

For each mail event, xapsd sends Apple the APNS device token, account ID, and credential topic
through TLS. Apple can observe notification timing and the sending server's IP address.
The software does not send email message content to Apple.

Never include keys, device tokens, account data, registration databases, email data, or private
server details in issues or documentation examples.
