# WebsitePublisher AI — MCP Server

**Your AI Builds and Publishes Websites & Apps — You Just Talk**

Your AI builds it, and it runs: live on your own domain with login, payments, forms, email and data from day one.

WebsitePublisher is the AI web platform. Connect the assistant you already use — Claude, ChatGPT, Gemini, Grok, Mistral, Cursor, GitHub Copilot, Devin Desktop (formerly Windsurf), Cline and others — and it builds and publishes real websites and web apps on a live URL. Pages, data, forms, visitor login, scheduled tasks and 133 integrations are MCP tools, with credentials in an encrypted vault the AI never sees. Your project has a memory across sessions and assistants: start in Cline, continue in Claude or ChatGPT months later, and the context is still there.

[![MCP](https://img.shields.io/badge/MCP-2025--11--25-blue)](https://modelcontextprotocol.io)
[![OAuth 2.1](https://img.shields.io/badge/Auth-OAuth%202.1-green)](https://auth.websitepublisher.ai/.well-known/oauth-authorization-server)
[![Tools](https://img.shields.io/badge/Tools-45-orange)](#-tools)
[![Integrations](https://img.shields.io/badge/Integrations-133-purple)](https://www.websitepublisher.ai/docs/integrations)
[![License](https://img.shields.io/badge/License-SaaS-lightgrey)](https://www.websitepublisher.ai/terms)

---

## ⚡ Quickstart

The server is remote and hosted: `https://mcp.websitepublisher.ai/`. Nothing to install, no API key. Every client logs in with OAuth on first use.

### Claude (claude.ai, Desktop, mobile)
WebsitePublisher is listed in Anthropic's Connectors Directory: one click, on every Claude plan, free included.
1. Open the [WebsitePublisher listing](https://claude.ai/directory/websitepublisher-ai), or in claude.ai go to **Settings → Connectors** and search for **WebsitePublisher**.
2. Click **Connect** and sign in. The tools are available in your chat right away.

### ChatGPT
1. Open the **WebsitePublisher** app in ChatGPT's app directory and connect it (free and Plus accounts work).
2. Start your message with **@WebsitePublisher**, for example: *"@WebsitePublisher build me a portfolio website"*.

### Cline
Add to `cline_mcp_settings.json`. Cline handles the OAuth login:
```json
{
  "mcpServers": {
    "WebsitePublisher": {
      "transport": { "type": "streamableHttp", "url": "https://mcp.websitepublisher.ai/" }
    }
  }
}
```
After saving, open Cline's MCP servers panel and click **Connect** next to WebsitePublisher, then sign in when the browser opens. Cline shows the tools only after that login. See [llms-install.md](llms-install.md) for agent-driven setup.

### Cursor
`~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):
```json
{
  "mcpServers": {
    "websitepublisher": { "url": "https://mcp.websitepublisher.ai/" }
  }
}
```
Restart Cursor, switch to Agent mode, sign in when the browser opens.

### GitHub Copilot (VS Code 1.101+)
`.vscode/mcp.json` — note `servers` and `"type": "http"`:
```json
{
  "servers": {
    "websitepublisher": { "type": "http", "url": "https://mcp.websitepublisher.ai/" }
  }
}
```

### Devin Desktop (formerly Windsurf)
`~/.config/devin/mcp_config.json` (Windows: `%APPDATA%\devin\mcp_config.json`):
```json
{
  "mcpServers": {
    "websitepublisher": { "serverUrl": "https://mcp.websitepublisher.ai/" }
  }
}
```
Restart, then enable the server in the Cascade panel (… → MCPs).

### Mistral / Le Chat
In Le Chat open **Intelligence → Connectors → + Add Connector**, search for **WebsitePublisher** in the directory, click **Add** and sign in. Enable it under the Tools button in your chat.

### Gemini
On gemini.google.com open **Settings → Connected Apps** and add a custom app with URL `https://mcp.websitepublisher.ai`. (Google currently limits custom apps to personal US accounts with Gemini in English; Workspace admins can add it for their organisation.)

### Grok
On grok.com open **Settings → Connectors** and add a custom connector with URL `https://mcp.websitepublisher.ai`.

### Any other MCP client
Clients with remote Streamable HTTP and OAuth: point them at `https://mcp.websitepublisher.ai/`. Clients that only run local (stdio) servers can use the `mcp-remote` bridge (Node.js 18+):
```json
{
  "mcpServers": {
    "websitepublisher": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.websitepublisher.ai/"]
    }
  }
}
```

**Quick test:** ask your AI *"List my WebsitePublisher projects"*. If that works, you're connected.

---

## 🔑 Authentication

No API keys. WebsitePublisher uses OAuth 2.1 with automatic discovery:

1. The client connects to `https://mcp.websitepublisher.ai/` and receives `401` with discovery metadata ([RFC 9728](https://www.rfc-editor.org/rfc/rfc9728)).
2. It registers itself via Dynamic Client Registration ([RFC 7591](https://www.rfc-editor.org/rfc/rfc7591)).
3. A browser opens: sign in with Google or email (one-time code).
4. The client receives its tokens and refreshes them automatically.

PKCE (S256) is required on all flows. Never share the `oauth` section a client writes to its config file: it holds personal tokens.

---

## 🛠 Tools

45 tools. Every write is live immediately at the project's URL; there is no separate deploy step.

### Start & projects
| Tool | Description |
|------|-------------|
| `get_started` | Call first: your projects with live URLs, which one to build in, and the exact next call |
| `get_skill` | Build patterns, design guidelines and the go-live checklist |
| `list_projects` | All projects you have access to |
| `get_project_status` | Project details: page, asset and entity counts, plan limits |
| `create_project` | Create a project (live at `project{ID}.websitepublisher.ai`; link your own domain in the dashboard) |

### Pages & versions
| Tool | Description |
|------|-------------|
| `list_pages` | Compact page index (no content) |
| `get_page` | One page with content and version hash |
| `create_page` | Publish a new page — live the moment the call returns |
| `update_page` | Replace a page (optimistic lock on version hash) |
| `patch_page` | Targeted find/replace edits without resending the page |
| `delete_page` | Delete a page (30 days in the project trash) |
| `get_page_versions` | Version history |
| `rollback_page` | Restore an earlier version |
| `fragments` | Shared HTML blocks (nav, footer, banners): list, create, update, patch, delete |

### Assets
| Tool | Description |
|------|-------------|
| `list_assets` | Images, CSS, JS, fonts, PDFs |
| `get_asset` | Read an asset's content |
| `upload_asset` | Upload via base64, plain text or public URL |
| `patch_asset` | Find/replace edits in CSS, JS, JSON or SVG |
| `delete_asset` | Delete an asset |

### Data (MAPI)
| Tool | Description |
|------|-------------|
| `entities` | Data models: list, create, update, delete, schema, properties, public read |
| `records` | Rows in an entity: list, get, create, update, delete |

### Vault
| Tool | Description |
|------|-------------|
| `vault_list_secrets` | Stored secrets — metadata only, never values |
| `vault_store_secret` | Store an encrypted secret (AES-256-GCM, write-only) |
| `vault_delete_secret` | Permanently delete a secret |

### Integrations
| Tool | Description |
|------|-------------|
| `search_integrations` | Find a capability in plain language, across all your projects |
| `list_integrations` | Integrations of a project and whether they are configured |
| `get_integration_schema` | Endpoints, inputs and usage guidance of one integration |
| `setup_integration` | Configure an integration (keys go to the vault) |
| `execute_integration` | Run an integration action — credentials resolved server-side |
| `remove_integration` | Remove an integration and its secrets |

### Forms, visitor login & analytics
| Tool | Description |
|------|-------------|
| `configure_form` | Define a form and its server-side actions (leads, email, webhook) |
| `list_forms` | Configured forms |
| `remove_form` | Remove a form |
| `configure_visitor_auth` | Visitor login by magic link or 6-digit code |
| `get_visitor_auth_config` | Current visitor login settings |
| `get_analytics` | Pageviews, visitors, referrers, devices, UTM, trend |

### Tracking
| Tool | Description |
|------|-------------|
| `set_tracking_scripts` | GA, GTM, Meta Pixel or other scripts on every page |
| `get_tracking_scripts` | Current tracking scripts |
| `remove_tracking_scripts` | Remove all tracking scripts |

### Visual editor
| Tool | Description |
|------|-------------|
| `create_edit_session` | Browser-based editor link so the user can place and edit images |
| `get_edit_session_changes` | What the user changed in the editor |

### Scheduled tasks & project memory
| Tool | Description |
|------|-------------|
| `create_scheduled_task` | Run a task on a cron schedule or once |
| `list_scheduled_tasks` | Tasks with status and next run |
| `delete_scheduled_task` | Delete a task |
| `tasks` | Project memory: tasks with append-only history, across sessions and assistants |

Integrations add no extra tools: their endpoints run through `execute_integration`. Browse all 133 at [websitepublisher.ai/docs/integrations](https://www.websitepublisher.ai/docs/integrations) or the machine-readable [integrations.txt](https://www.websitepublisher.ai/integrations.txt).

---

## 📋 Technical details

| | |
|---|---|
| **Server URL** | `https://mcp.websitepublisher.ai/` |
| **Transport** | Streamable HTTP (JSON-RPC 2.0) |
| **Protocol** | MCP 2025-11-25 |
| **Auth discovery** | `https://mcp.websitepublisher.ai/.well-known/oauth-protected-resource` |
| **Auth server** | `https://auth.websitepublisher.ai/.well-known/oauth-authorization-server` |
| **PKCE** | Required (S256) |
| **Scopes** | `mcp:read`, `mcp:write`, `mcp:full` |

---

## 💰 Pricing

Prices in euros, excluding VAT. Live: [websitepublisher.ai/pricing](https://www.websitepublisher.ai/pricing).

| Plan | Price | Projects | Pages per project |
|------|-------|----------|-------------------|
| **Free** | €0 | 1 | 5 |
| **Solo** | €4/mo | 1 | 15 |
| **Starter** | €9/mo | 3 | 30 |
| **Pro** | €19/mo | 10 | 100 |
| **Agency** | €49/mo | 30 | 300 |

---

## 📖 Documentation

- **MCP setup per AI client:** [websitepublisher.ai/docs/mcp](https://www.websitepublisher.ai/docs/mcp)
- **Quick start:** [websitepublisher.ai/docs/quickstart](https://www.websitepublisher.ai/docs/quickstart)
- **Pages & assets (PAPI):** [websitepublisher.ai/docs/papi](https://www.websitepublisher.ai/docs/papi)
- **Data (MAPI):** [websitepublisher.ai/docs/mapi](https://www.websitepublisher.ai/docs/mapi)
- **Forms & visitor login (SAPI):** [websitepublisher.ai/docs/sapi](https://www.websitepublisher.ai/docs/sapi)
- **Integrations:** [websitepublisher.ai/docs/integrations](https://www.websitepublisher.ai/docs/integrations)
- **For AI agents:** [llms.txt](https://www.websitepublisher.ai/llms.txt) · [llms-full.txt](https://www.websitepublisher.ai/llms-full.txt)
- **Dashboard:** [dashboard.websitepublisher.ai](https://dashboard.websitepublisher.ai)

---

## 🔒 Security & privacy

- OAuth 2.1 with PKCE on all flows, Dynamic Client Registration — no pre-shared secrets
- Credentials for integrations live in an encrypted vault (AES-256-GCM); the AI never sees the values
- Rate limiting on all endpoints
- [Privacy policy](https://www.websitepublisher.ai/privacy) · [Terms](https://www.websitepublisher.ai/terms)

---

## 🏗 This repository

This is the documentation and integration repository for the WebsitePublisher AI MCP server. The server is a hosted service: no installation or self-hosting. Connect your AI client to `https://mcp.websitepublisher.ai/` and start building.

---

## 📬 Support

- **Docs:** [websitepublisher.ai/docs](https://www.websitepublisher.ai/docs)
- **Email:** support@websitepublisher.ai
- **Website:** [websitepublisher.ai](https://www.websitepublisher.ai)

---

<p align="center">
  <strong>WebsitePublisher AI</strong> — Your AI Builds and Publishes Websites & Apps — You Just Talk
</p>
