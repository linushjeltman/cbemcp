# xios-docs

The XIOS/3 documentation MCP server. It answers questions about the framework and nothing
else, so it is useful on its own, whether or not you deploy anything:

`xios_concepts`, `xios_list`, `xios_component`, `xios_operation`, `xios_function`,
`xios_method`, `xios_event`, `xios_action`, `xios_icons`.

## Installing

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install xios-docs@cloudbackend
/mcp
```

`/mcp` runs the OAuth login for this server only. It is protected by the same Keycloak as
CloudBackend PaaS, so you need an account there even though the tools are read-only
documentation.

If you already have `xios` configured by hand, remove it after installing, or the same
tools show up twice:

```bash
claude mcp remove xios -s user
```

To upload and deploy what you build with it, install `cloudbackend-paas` as well. That
plugin also carries the `xios-apps` skill, which knows how the two fit together.

## Pointing at another server

```
XIOS_MCP_URL=https://mcp.xios3.com/mcp
```

Set it in your shell or in the `env` block of `~/.claude/settings.json`. Only useful for
testing against a non-production documentation server.
