# cloudbackend-paas

The CloudBackend PaaS MCP server, plus a skill for authoring XIOS/3 applications:

- `upload_project` — write a project into the database, validating it against the schema
- `deploy` — publish an uploaded project
- the `xios-apps` skill — project layout, the manifest, and the rules that make a deploy
  fail

The documentation tools are a separate install. If you want Claude to look up how a
component or an operation works while it writes the XML, install `xios-docs` as well; the
two are independent and each has its own login.

## Installing

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install cloudbackend-paas@cloudbackend
/mcp
```

Set `CBE_TENANT` first, see below. `/mcp` runs the OAuth login for this server only.

If you already have `cbemcp` configured by hand, remove it after installing, or the same
tools show up twice:

```bash
claude mcp remove cbemcp -s user
```

## Which tenant

The server URL carries the tenant, so the plugin reads it from the environment:

```
https://mcp.cloudbackend.com/mcp/${CBE_TENANT:-cbetesttenantshopfinity}
```

Set `CBE_TENANT` to your own tenant, in your shell or in the `env` block of
`~/.claude/settings.json`. The tenant selects the Keycloak realm you log in to, so on the
wrong one the login fails rather than writing anywhere unexpected. The default is the test
tenant and is there so the plugin works before it is configured, not because it is the
right tenant for anyone.

## For a whole team, without anyone typing commands

Declare the marketplace and the plugin in managed or user settings instead:

```json
{
  "extraKnownMarketplaces": {
    "cloudbackend": {
      "source": {
        "source": "github",
        "repo": "linushjeltman/cbemcp"
      }
    }
  },
  "enabledPlugins": {
    "cloudbackend-paas@cloudbackend": true,
    "xios-docs@cloudbackend": true
  }
}
```

Everyone who gets that settings file has the plugins on their next session. They still
need their own `CBE_TENANT`, and they still log in to each server separately.
