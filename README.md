# ZERNO Lite — MCP Server

Official metadata for the **ZERNO Lite** Model Context Protocol (MCP) server.

ZERNO Lite is a **remote MCP server** that connects AI agents to ZERNO project context —
project briefs, task and agent context, and structured intake workflows — over a secure,
OAuth-authorized endpoint. It's built for coding agents, product agents, and automation
workflows that need to understand a project before acting, without unrestricted backend access.

> This repository is the public "front door" for the hosted server: metadata, `server.json`, and the
> tool catalog for MCP directories. **It does not contain the backend source** — the server runs as a
> hosted service at the endpoint below.

## Endpoint
```
https://zerno.one/mcp
```

| | |
|---|---|
| **Transport** | Streamable HTTP (JSON-RPC 2.0 over POST) |
| **Auth** | OAuth 2.1 with PKCE (S256); bearer tokens; per-project authorization |
| **Registration** | Dynamic Client Registration (RFC 7591) — no static API key |
| **Capabilities** | 8 tools · 0 resources · 0 prompts |
| **Registry name** | `one.zerno/zerno-lite` |
| **Manifest** | https://zerno.one/.well-known/mcp |

## Quickstart
Add a remote connector named `zerno` with URL `https://zerno.one/mcp`, then complete the OAuth
consent in the browser.

- **Claude Code / Claude Desktop:** Settings → Connectors → add custom connector (name `zerno`, URL above).
- **Codex CLI:** `codex mcp add zerno --url https://zerno.one/mcp`
- **Cursor:** Settings → MCP → Add server → URL `https://zerno.one/mcp`

For user-scoped tokens, tell the agent which project with a `project_slug` (e.g. `zerno-one`).

## Tools (8)
| Tool | Does | Writes? |
|---|---|---|
| `zerno_get_project_brief` | Project orientation: focus, next action, do-not-do, memory index | no |
| `zerno_get_agent_context` | Compiled project memory + recent events for an agent run | no |
| `zerno_list_tasks` | Filtered backlog listing | no |
| `zerno_get_task` | Full detail for one task | no |
| `zerno_update_task` | Patch task fields/status (ICE recomputed) | mutates |
| `zerno_submit_task_triage` | Submit triage proposals (pending human approval) | creates proposals |
| `zerno_create_agent_intake_task` | File a review task from an agent report | creates a task |
| `zerno_capture_session_event` | Append a session summary to project memory | creates memory event |

Full machine-readable schemas: [`tools-list.json`](./tools-list.json). No tool deletes data or calls
third-party services. `tools/list` is scope-filtered — a read-only token sees only the four read tools.

## Security & privacy
Every call requires OAuth; nothing is exposed unauthenticated. Bearer tokens are hashed at rest, no
secrets are returned in tool output, and every call is audit-logged. Access is tied to the user's
authorized ZERNO workspace/project.

## Limitations
Requires a ZERNO account and an authorized project. Remote/hosted only (no stdio or packaged
distribution). Tools only — no MCP resources or prompts.

## Files in this repo
- [`server.json`](./server.json) — Official MCP Registry manifest
- [`tools-list.json`](./tools-list.json) — full `tools/list` (8 tools, exact schemas)
- [`smithery-server-card.json`](./smithery-server-card.json) — static server card for Smithery
- [`LICENSE`](./LICENSE) — MIT

## Links
- Website: https://zerno.one
- Docs: https://zerno.one/docs/mcp

## License
[MIT](./LICENSE) — applies to this metadata repository.
