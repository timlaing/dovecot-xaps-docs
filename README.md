# Dovecot XAPS documentation

Combined administrator documentation for the Dovecot XAPS plugin, daemon, and signed APT repository.

Read the guide at <https://dovecot-xaps.readthedocs.io/>.

## Local preview

```sh
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
mkdocs serve
```

Run `mkdocs build --strict` before submitting changes. See [contributing](docs/contributing.md)
for ownership, publishing, and Read the Docs setup.
