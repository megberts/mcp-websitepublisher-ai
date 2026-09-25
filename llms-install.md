# WebsitePublisher.ai MCP Server — Installation Guide for Cline

This guide is written for an AI agent (Cline) installing the WebsitePublisher.ai MCP server.

## What this server is

WebsitePublisher.ai is a **remote, hosted MCP server**. There is nothing to clone, build or run locally.

- Endpoint: `https://mcp.websitepublisher.ai/`
- Auth: OAuth 2.1 with Dynamic Client Registration and PKCE
- API keys: none. Do not ask the user for an API key or token.

## Prerequisites

- Node.js 18 or newer, so that `npx` is available. Check with `node --version`.
- A browser on the user's machine. The first connection opens a login page.

## Installation

Add the following entry to `cline_mcp_settings.json`, under `mcpServers`. Keep any servers that are already there.

```json
{
  "mcpServers": {
    "websitepublisher": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.websitepublisher.ai/"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

`mcp-remote` bridges the remote server to stdio and handles the OAuth flow.

## First connection (user action required)

1. After you save the settings, Cline starts the server. A browser window then opens at `auth.websitepublisher.ai`.
2. The user logs in with Google or with email (a one-time code). Tell the user this is expected.
3. After login, the browser redirects back and the connection is complete. The token is cached, so later sessions do not open the browser again.

Do not try to complete the login on the user's behalf.

## Verify the installation

1. Call `get_started` with no arguments. It returns the user's project and the recommended next call.
2. Optionally, call `list_projects`. A response that lists projects, or an empty list for a new account, means the setup works.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `npx: command not found` | Install Node.js 18+ and restart VS Code. |
| The browser does not open | Restart the server from Cline's MCP Servers panel. `mcp-remote` prints the login URL in the server log, so it can also be opened manually. |
| `401` / `invalid_token` after it worked before | Clear the cached token with `rm -rf ~/.mcp-auth`, then restart the server and log in again. |
| Tools not listed | Check that the URL ends with `/` exactly as shown above, then restart the server. |

## Usage notes for the agent

- Every write is live immediately at the project's URL. There is no separate deploy step.
- Before building or changing a site, call `get_skill`. Then call `get_skill` with `skill_name: "design"`.
- Store credentials with the vault tools only. Never put them in page code.

More: https://www.websitepublisher.ai/docs/mcp
