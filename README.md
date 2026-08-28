# CloudBackend Claude Code plugins

A Claude Code marketplace with one plugin, `cloudbackend-xios`, which connects both MCP
servers a XIOS/3 application needs and ships a skill for the workflow between them:

- CloudBackend PaaS — `upload_project` and `deploy`
- XIOS/3 documentation — `xios_component`, `xios_operation`, `xios_concepts` and the rest

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install cloudbackend-xios@cloudbackend
/mcp
```

See [plugins/cloudbackend-xios/README.md](plugins/cloudbackend-xios/README.md) for the
tenant setting and for installing it across a team.
