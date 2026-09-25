# WebsitePublisher.ai MCP Server — Installation Guide for Cline

This guide is written for an AI agent (Cline) installing the WebsitePublisher.ai MCP server.

## What this server is

WebsitePublisher.ai is a **remote, hosted MCP server**. There is nothing to clone, build or run locally.

- Endpoint: `https://mcp.websitepublisher.ai/`
- Auth: OAuth 2.1 with Dynamic Client Registration and PKCE
- API keys: none. Do not ask the user for an API key or token.

## Prerequisites

- A browser on the user's machine. The first connection opens a login page.
- Local port `1456` must be free. Cline receives the OAuth callback on `http://127.0.0.1:1456/mcp/oauth/callback`.

## Installation (native remote connection, recommended)

Add the following entry to `cline_mcp_settings.json`, under `mcpServers`. Keep any servers that are already there.

```json
{
  "mcpServers": {
    "WebsitePublisher": {
      "transport": {
        "type": "streamableHttp",
        "url": "https://mcp.websitepublisher.ai/"
      }
    }
  }
}
```

Write only the `transport` block. Cline adds the `oauth` section on its own after the user logs in. Never copy an `oauth` section from another machine or from documentation: it holds personal tokens.

## First connection (user action required)

1. After you save the settings, Cline discovers the auth server and registers itself as a client. A browser window then opens at `auth.websitepublisher.ai`.
2. The user logs in with Google or with email (a one-time code). Tell the user this is expected.
3. After login, the browser redirects back to Cline and the connection is complete. Cline stores the tokens and refreshes them automatically.

Do not try to complete the login on the user's behalf.

## Alternative: via mcp-remote

Use this only if the native connection is not available in the user's Cline version. It requires Node.js 18 or newer, so that `npx` is available.

```json
{
  "mcpServers": {
    "WebsitePublisher": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.websitepublisher.ai/"]
    }
  }
}
```

## Verify the installation

1. Call `get_started` with no arguments. It returns the user's project and the recommended next call.
2. Optionally, call `list_projects`. A response that lists projects, or an empty list for a new account, means the setup works.

## Troubleshooting

| Symptom | Fix |
|---|---|
| The browser does not open | Restart the server from Cline's MCP Servers panel. |
| The OAuth callback fails | Check that nothing else is listening on port `1456`. |
| `401` / `invalid_token` after it worked before | Remove the `oauth` section from this server's entry, save the file, then log in again. With mcp-remote, clear `~/.mcp-auth` instead. |
| `npx: command not found` (mcp-remote only) | Install Node.js 18+ and restart the editor. |
| Tools not listed | Check that the URL ends with `/` exactly as shown above, then restart the server. |

## Usage notes for the agent

- Every write is live immediately at the project's URL. There is no separate deploy step.
- Before building or changing a site, call `get_skill`. Then call `get_skill` with `skill_name: "design"`.
- Store credentials with the vault tools only. Never put them in page code.

More: https://www.websitepublisher.ai/docs/mcp
