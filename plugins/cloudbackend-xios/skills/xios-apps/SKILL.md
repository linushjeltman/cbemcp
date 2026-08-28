---
name: xios-apps
description: Build a XIOS/3 application and get it onto CloudBackend PaaS - project layout, looking up components and operations before writing XML, then upload_project and deploy. Use whenever the task involves XIOS/3 views, processes, an application manifest (index.xml), or uploading or deploying a project to CloudBackend PaaS.
---

# XIOS/3 applications

Two MCP servers cover this work and they are used in this order:

- `xios` — documentation. `xios_concepts`, `xios_list`, `xios_component`, `xios_operation`,
  `xios_function`, `xios_method`, `xios_event`, `xios_action`, `xios_icons`.
- `cbemcp` — CloudBackend PaaS. `upload_project` writes the project into the database,
  `deploy` publishes it.

## Look it up, don't guess

XIOS/3 is XML with a schema behind it, and attribute names do not follow from the tag
names. Before writing markup for a component you have not used in this session, call
`xios_component` for it and use the attributes it lists. The same goes for `xios_operation`
before writing a process step.

Useful entry points: `xios_list` with `components`, `operations`, `functions`, `methods`,
`events`, `actions` or `concepts` returns just the names, and `xios_concepts` takes topics
such as `ui layout and views`, `process logic`, `data binding`, `aliases (variables)`,
`expressions`, `rules`, `events`, `components`, `component patterns`, `styling and themes`
and `advanced patterns`.

## Project layout

```
index.xml              the application manifest, root tag <application>
views/mainView.xml     root tag <view>
processes/main.xml     root tag <process>
locale/en_US.xml       optional
```

The manifest is what ties it together, and references are relative to the project root:

```xml
<?xml version="1.0" encoding="utf-8"?>
<application name="Hello World" xmlns:xlink="http://www.w3.org/1999/xlink">
  <about author="..." version="1.0">
    <description>What this application is</description>
  </about>
  <view xlink:type="simple" xlink:actuate="onLoad" xlink:href="views/mainView.xml"/>
  <process xlink:type="simple" xlink:actuate="onLoad" xlink:href="processes/main.xml"/>
</application>
```

A view holds one `<panel>` (plus optional menus and toolbars before it), and panels nest:
`type="rows"` stacks, `type="columns"` lays out horizontally, sizes are px, `%`, `auto` or
`fill`. Give the view a `name`, `width` and `height`, and give every component a `name` so
processes can address it.

A process is triggers, steps and operations. A trigger names a view, a component and an
event and activates a step by id; the step's operations run in order:

```xml
<process name="main">
  <trigger view="helloView" component="greetButton" event="Select" step="greet"/>
  <step id="greet">
    <operation name="callMethod" value="#helloView#greeting">
      <method name="setValue"><param type="string">Hello again</param></method>
    </operation>
  </step>
</process>
```

`step id="start"` runs before any view loads, so it cannot touch components; use a view's
`Loaded` event for anything that references them. Prefer actions over `callMethod` when an
action exists for what you want, they are faster.

## Upload and deploy

`upload_project` takes a `project_name` and a list of files with paths relative to the
project root. It writes to `home://Applications/<project_name>/` by default, or
`tenant://Applications/<project_name>/` with `destination_db: "tenant"` for a project
shared with the tenant. Files that already exist are overwritten and files that are not
sent are left alone, so a fix can be a single file rather than the whole project.

It validates by default: the manifest and everything reachable from it through
`xlink:href` are checked against the schema, and other referenced files have to exist. A
failure names the file and the reason, so fix and re-upload rather than deploying anyway.

`deploy` then publishes it. Two things to know:

- `deployment_name` may only contain lowercase alphanumeric characters. If the project
  name has capitals, a hyphen or a space, pass `deployment_name` explicitly, eg. project
  `HelloWorld` deploys as `helloworld`.
- Pass `source_db` only when the same project name exists in both `home://Applications/`
  and `tenant://Applications/`.

Deploying requires `index.xml` with an `<application>` root tag; a project without one is
just files in the database.
