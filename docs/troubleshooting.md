# Troubleshooting

## Connection refused

Check the endpoint on both sides. `localhost` may resolve to `127.0.0.1`, while xapsd defaults to
`[::1]:11619`. Use `xaps_url = http://[::1]:11619` explicitly for the default installation.
Confirm xapsd is running and check the listener with `sudo ss -ltnp | grep 11619`.

## Dovecot fails to load a plugin or setting

Check the installed Dovecot version, package architecture, and APT suite. Use the matching
[configuration syntax](configuration/dovecot.md), preserve plugin order, and check for duplicate
configuration. Run `sudo doveconf -n` before restarting. A macOS build does not establish compatibility
with the deployed Dovecot ABI.

## Registration succeeds but no push arrives

Confirm mail delivery uses LDA or LMTP and the relevant protocol loads the push-notification plugins.
Check xapsd logs for APNS errors, credential access problems, or rejected key/topic combinations.
Confirm outbound connectivity to APNS. Reconnect iOS Mail and test again on the physical device.
Registration success and unit-test success do not prove end-to-end delivery.

## Only one migrated device receives notifications

If two devices share registration/account data following migration, remove and re-add the mail account
on one device. Back up data first and understand the effect on locally stored mail. Do not delete the
entire registration database as a first troubleshooting step.

## APT errors

Check the suite, architecture, signing key, and system clock. Review `apt-cache policy` output.
Do not disable signature verification to work around a signing error. The repository homepage
redirect does not change `/dists/`, `/pool/`, or `/public-key.asc` package URLs.

## Ask for help

Report plugin/configuration problems to the
[plugin tracker](https://github.com/timlaing/dovecot-xaps-plugin/issues), daemon/APNS problems to the
[daemon tracker](https://github.com/timlaing/dovecot-xaps-daemon/issues), and package-index problems to the
[APT tracker](https://github.com/timlaing/dovecot-xaps-apt/issues).
Include component versions, operating system, architecture, and redacted error messages.
