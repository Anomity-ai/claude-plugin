# Changelog

Notable changes to the Anomity plugin for Claude. Version numbers follow the
`version` field in `.claude-plugin/plugin.json`.

## 0.1.0 (unreleased)

First version, submitted to the Claude directory on 2026-10-02.

- Seven skills on top of the Anomity connector: `posture-briefing`,
  `triage-findings`, `investigate-device`, `review-mcp-servers`,
  `review-allowlist`, `audit-changes`, and `compliance-readiness`.
- A connector entry for `https://app.anomity.ai/mcp`, with the public client ID
  and the sign-in port pinned so Claude Code connects without extra setup.
- Every skill treats what the connector returns as data, never as
  instructions.
