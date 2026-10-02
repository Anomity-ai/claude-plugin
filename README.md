# Anomity for Claude

Anomity gives security teams one view of every AI tool running on their managed
devices: coding agents, MCP servers, skills and instruction files, browser AI use,
personal versus corporate AI accounts, and the credentials those tools can reach.
Endpoint sensors deployed through your MDM report that inventory to the Anomity
console, where policies turn it into findings.

This plugin brings that data into Claude. It connects to your organization's
Anomity console through the Anomity connector, and adds skills that teach Claude
the jobs a security team actually does with it, so that a question like "brief me
on our AI risk this week" comes back as a structured briefing built from live
data, not a guess assembled from tool names.

## What you can ask

| Skill | Ask something like |
|---|---|
| `posture-briefing` | "Give me an AI security briefing for the CISO." |
| `triage-findings` | "What should we fix first among our open findings?" |
| `investigate-device` | "What's going on with Dana's laptop?" |
| `review-mcp-servers` | "Which MCP servers on our fleet should worry us?" |
| `review-allowlist` | "Walk me through what's waiting for approval on the allowlist." |
| `audit-changes` | "What changed in our AI tooling last week?" |
| `compliance-readiness` | "Where are we failing SOC 2 and the EU AI Act, and why?" |

In Claude Code, each skill is also available as a command, for example
`/anomity:posture-briefing`.

## Requirements

- An Anomity organization, and a user in it. You sign in with the same account
  you use for the Anomity console, the same way you sign in there.
- What you can see follows your console role. Every role can read posture,
  findings and inventory; the audit trail needs the analyst role.
- Approving allowlist items from Claude is off by default. It needs an admin, and
  an owner must first turn on **Settings > AI Assistant Connector > Allow AI
  assistants to approve allowlist items** in the console. Claude asks you to
  confirm the exact items before every approval.

## Set it up

**Claude (web, desktop, mobile) and Cowork.** Install the plugin, then open its
**Connectors** tab and connect **Anomity**. On Team and Enterprise plans, an
Owner adds the Anomity connector for the organization first, and each member
then connects with their own Anomity account. The plugin points at the same
server as the Anomity connector in the directory, so if you already have that
connector, Claude sees one set of Anomity tools, not two.

**Claude Code.** The plugin's connector entry already carries the Anomity
client ID and the fixed sign-in port Anomity's login service expects, so there
is nothing to configure. Run `/mcp`, choose the Anomity server, and
authenticate.

Full connection instructions are in the
[Anomity docs](https://anomity.ai/docs/#the-anomity-mcp-server).

## What this plugin contains, runs, and sends

- **Skills only.** The plugin is Markdown instructions plus one connector
  reference (`.mcp.json`). It contains no hooks, agents, scripts, executables, or
  local servers, and it runs no code on your machine.
- **One connector, one destination.** Every request goes to the Anomity
  connector at `https://app.anomity.ai/mcp`, over your own sign-in. The plugin
  sends nothing anywhere else and fetches nothing else.
- **Read-only by default.** Every connector tool reads your organization's
  existing Anomity data. The only tool that changes anything, approving
  allowlist items, exists only when your owner turned it on and you are an
  admin.
- **No secret values.** Anomity redacts credentials on the device before they
  leave it. The connector reports that a secret was found, where, and how
  severe, never the value itself.
- **Scoped to your organization and your role.** The connector cannot reach
  another organization's data or show more than your console role allows.

The `oauth.clientId` in `.mcp.json` identifies the public Anomity connector
application to Anomity's login service. It is not a secret, carries no
permissions, and grants nothing until you sign in.

## Support and privacy

- Documentation: [anomity.ai/docs](https://anomity.ai/docs/#the-anomity-mcp-server)
- Support: [support@anomity.ai](mailto:support@anomity.ai)
- Privacy policy: [anomity.ai/legal/privacy-policy](https://anomity.ai/legal/privacy-policy/)
- Terms of service: [anomity.ai/legal/terms-of-service](https://anomity.ai/legal/terms-of-service/)

## License

[MIT](LICENSE)
