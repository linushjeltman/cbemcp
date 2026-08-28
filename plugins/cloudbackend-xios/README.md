# cloudbackend-xios

One Claude Code plugin that connects both servers a XIOS/3 application needs, and a skill
that knows how the two are used together:

- `cbemcp` — CloudBackend PaaS: `upload_project` and `deploy`.
- `xios` — the XIOS/3 documentation server: `xios_component`, `xios_operation`,
  `xios_concepts` and the rest.

Both are remote MCP servers behind Keycloak, so Claude Code runs an OAuth login for each
the first time they are used. Same authorization server for both, so the second login is
usually one click.

## Installing

In Claude Code:

```
/plugin marketplace add linushjeltman/cbemcp
/plugin install cloudbackend-xios@cloudbackend
/mcp
```

The first command registers this repo as a marketplace, the second installs the plugin
from it, and `/mcp` runs the OAuth login for `cbemcp` and `xios`. Set `CBE_TENANT` to your
tenant first, see below.

If you already have `cbemcp` or `xios` configured by hand, remove them after installing,
or the same tools show up twice:

```bash
claude mcp remove cbemcp -s user
claude mcp remove xios -s user
```

### For a whole team, without anyone typing commands

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
    "cloudbackend-xios@cloudbackend": true
  }
}
```

Everyone who gets that settings file has the plugin on their next session, with no commands
to run and nothing to remember. They still need their own `CBE_TENANT`.

## Which tenant

The `cbemcp` URL carries the tenant, so the plugin reads it from the environment:

```
https://mcp.cloudbackend.com/mcp/${CBE_TENANT:-cbetesttenantshopfinity}
```

Set `CBE_TENANT` to your own tenant, in your shell or in the `env` block of
`~/.claude/settings.json`. The default is the test tenant and is there so the plugin works
before it is configured, not because it is the right tenant for anyone.

`XIOS_MCP_URL` overrides the documentation server the same way, which is only useful when
pointing at a non-production one.

## What the skill does

`skills/xios-apps/SKILL.md` covers the parts that are easy to get wrong: project layout
and what belongs in the manifest, looking components and operations up before writing XML
rather than guessing attribute names, and the two rules that make `deploy` fail
(`deployment_name` has to be lowercase alphanumeric, and the project needs an
`<application>` root in `index.xml`). Claude loads it when a task involves XIOS/3 views,
processes or a deploy.
