# CloudBackend Claude Code plugins

A Claude Code marketplace with two plugins. They are independent: install either or both,
and each logs in to its own MCP server.

- **cloudbackend-paas** — CloudBackend PaaS: `upload_project` and `deploy`, plus the
  `xios-apps` skill for authoring XIOS/3 applications.
- **xios-docs** — the XIOS/3 documentation server: `xios_component`, `xios_operation`,
  `xios_concepts` and the rest. Useful on its own.

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install cloudbackend-paas@cloudbackend
/plugin install xios-docs@cloudbackend
/mcp
```

`cloudbackend-paas` needs `CBE_TENANT` set to your tenant, see
[its README](plugins/cloudbackend-paas/README.md).
