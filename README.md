# CloudBackend Claude Code plugins

A Claude Code marketplace with one plugin, **cloudbackend-paas**: everything needed to
build a XIOS/3 application and get it onto CloudBackend PaaS.

- `upload_project` and `deploy` — write a project into the database and publish it
- the XIOS/3 documentation lookups — `xios_component`, `xios_operation`, `xios_concepts`
  and the rest, so the XML is looked up rather than guessed
- the `xios-apps` skill — project layout, the manifest, and the rules that make a deploy
  fail

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install cloudbackend-paas@cloudbackend
/mcp
```

Restart Claude Code after installing, then `/mcp` to log in — once per server, since the
two servers are separate resources. Details, including why the connectors pin an OAuth
client, are in [the plugin README](plugins/cloudbackend-paas/README.md).

Proof of concept: the tenant is hardcoded to `cbetesttenantshopfinity`, the one Keycloak
realm where the OAuth client is registered.
