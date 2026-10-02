---
name: audit-changes
description: Answer what changed in the organization's AI tooling or Anomity settings over a period, from the Anomity audit trail. Use when the user asks what changed, what's new on the fleet, who changed a policy or approved something, or when an MCP server, permission, or AI tool first appeared or disappeared.
---

# What changed

The audit trail holds the last 90 days of two kinds of events: changes the
Anomity sensors saw on devices, and actions people took in the console. Reading
it needs the analyst role; if the call is refused, tell the user that.

If the Anomity tools aren't available, tell the user to connect Anomity: from the
plugin's **Connectors** tab in Claude, or with `/mcp` in Claude Code.

## Query

Call `query_audit_trail` with:

- `from` and `to` in epoch milliseconds. Default to the last seven days. A window
  older than 90 days returns nothing.
- `limit: 500`. If you get exactly 500 rows back, the window holds more; narrow
  it or tell the user.
- `action`, `deviceId`, or `actorEmail` when the question is that specific.

## Read each row

Each row has `action`, `scope` (`device` or `cloud`), `timestamp` (epoch ms),
`deviceId`, `agentName` (the AI tool, on device rows), `actorEmail` (the person,
on cloud rows), and `diff`.

- **Device changes** (`scope: "device"`): `mcp_added`, `mcp_removed`,
  `mcp_modified`, `permission_changed`, `config_changed` (which also covers
  skills and hooks), `agent_removed` (an AI tool was removed), and
  `ssh_host_added`. The `diff` lists the changes, each with `path`, `type`
  (`added`, `removed`, `changed`), `oldValue`, and `newValue`; it can arrive as an
  object keyed `"0"`, `"1"`, and so on, which you read in key order.
- **Console actions** (`scope: "cloud"`): for example `policy_created`,
  `policy_updated`, `policy_toggled`, `violation_status_changed`,
  `mcp_approved`, `cli_approved`, `user_role_changed`, `org_settings_updated`,
  and `suppression_created`. When `diff.summary` is present, it is the
  one-sentence description the console shows; use it.
- `channel: "mcp"` marks an action taken through an AI assistant connector.
- `actorType: "platform"` marks an action Anomity support took on the
  organization's behalf; those rows carry no `actorEmail`.

To name devices, call `list_devices`: a row's `deviceId` matches a device's
`id`, except on `device_enrolled` and `device_re_enrolled` rows, where it matches
the device's `deviceId`. Show the device's `label`.

## Report

Group device changes by device and console actions by person. Lead with the
changes that matter for security: new MCP servers, widened permissions, removed
policies, approvals, and role changes. Collapse repetitive noise into a count.
Give dates in the user's time zone when you know it.

Console link: `https://app.anomity.ai/audit-trail`.
