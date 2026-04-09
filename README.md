# 🌐 WebsitePublisher.ai — MCP Server

**Build and publish websites through AI conversation.**

Create pages, upload assets, manage data, and deploy to custom domains — all from your favorite AI assistant. Just connect and start talking.

[![MCP](https://img.shields.io/badge/MCP-2025--06--18-blue)](https://modelcontextprotocol.io)
[![OAuth 2.1](https://img.shields.io/badge/Auth-OAuth%202.1-green)](https://auth.websitepublisher.ai/.well-known/oauth-authorization-server)
[![Tools](https://img.shields.io/badge/Tools-57-orange)](#tools)
[![License](https://img.shields.io/badge/License-SaaS-lightgrey)](https://websitepublisher.ai/terms)
[![Privacy](https://img.shields.io/badge/Privacy-Policy-blue)](https://websitepublisher.ai/privacy)

---

## ⚡ Quickstart

### Claude.ai (Connectors)
1. Go to **Settings → Connectors → Add custom connector**
2. Enter name: `WebsitePublisher` and URL: `https://mcp.websitepublisher.ai`
3. Click **Connect** → Browser opens for login → Done ✅

### Claude Desktop
Add to your `claude_desktop_config.json`:
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
Restart Claude Desktop → Browser opens for login → Done ✅

### Mistral / Le Chat (one-click)
1. Open Le Chat → Intelligence → Connectors → **+ Add Connector**
2. Choose **Custom MCP Connector**
3. Enter URL: `https://mcp.websitepublisher.ai/`
4. Click Connect → Login → Done ✅

### ChatGPT
Search **"WebsitePublisher"** in the GPT Store, or visit:
[chat.openai.com/g/websitepublisher](https://chat.openai.com)

### Cursor / Windsurf / Claude Code
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

---

## 🔑 Authentication

**Zero configuration required.** WebsitePublisher uses OAuth 2.1 with automatic discovery:

1. Your AI client connects to `https://mcp.websitepublisher.ai/`
2. Server responds with `401` + discovery metadata ([RFC 9728](https://www.rfc-editor.org/rfc/rfc9728))
3. Client auto-registers via Dynamic Client Registration ([RFC 7591](https://www.rfc-editor.org/rfc/rfc7591))
4. Browser opens → Login with Google or email (OTP) → Redirect back
5. Client receives OAuth token → Connected ✅

PKCE is enforced on all flows. No API keys to manage.

---

## 🛠 Tools

57 tools organized across twelve categories:

### 📁 Project Management
| Tool | Description |
|------|-------------|
| `get_skill` | Get the full agent skill document with patterns, snippets, and go-live checklist |
| `list_projects` | List all projects you have access to |
| `get_project_status` | Get project details: pages, assets, domain info |

### 📄 Pages
| Tool | Description |
|------|-------------|
| `list_pages` | List all pages in a project |
| `get_page` | Get page content with version info |
| `create_page` | Create a new HTML page |
| `update_page` | Replace full page content |
| `patch_page` | Apply targeted changes (diff-based, saves tokens) |
| `delete_page` | Delete a page |
| `get_page_versions` | View version history |
| `rollback_page` | Restore a previous version |

### 🧩 Fragments (Reusable Components)
| Tool | Description |
|------|-------------|
| `list_fragments` | List all reusable HTML fragments |
| `create_fragment` | Create a reusable fragment (nav, footer, banner) |
| `update_fragment` | Update a fragment — all pages using it update automatically |
| `delete_fragment` | Delete a fragment |

### 🖼 Assets
| Tool | Description |
|------|-------------|
| `list_assets` | List uploaded files (images, CSS, JS) |
| `upload_asset` | Upload a file via base64 or URL |
| `delete_asset` | Remove an asset |
| `patch_asset` | Apply text changes to CSS/JS/SVG assets without re-uploading |

### 📊 Entities (Structured Data)
| Tool | Description |
|------|-------------|
| `list_entities` | List data models in a project |
| `create_entity` | Create a new data model with properties |
| `get_entity_schema` | Get the schema/structure of an entity |
| `update_entity` | Update entity metadata or enable public read access |
| `delete_entity` | Remove an entity and all its data |

### 📝 Records (Data)
| Tool | Description |
|------|-------------|
| `list_records` | List records with pagination |
| `get_record` | Get a single record by ID |
| `create_record` | Create a new record |
| `update_record` | Update an existing record |
| `delete_record` | Delete a record |

### 🔐 Vault (Secrets Management)
| Tool | Description |
|------|-------------|
| `vault_list_secrets` | List stored secrets (metadata only, never values) |
| `vault_store_secret` | Store or update an encrypted secret (AES-256-GCM) |
| `vault_delete_secret` | Permanently delete a secret |

### 🔌 Integrations
| Tool | Description |
|------|-------------|
| `list_integrations` | List available integrations and their config status |
| `get_integration_schema` | Get full schema of an integration's endpoints and inputs |
| `setup_integration` | Configure an integration by storing API keys |
| `execute_integration` | Execute an integration action (email, payment, etc.) |
| `remove_integration` | Remove an integration and its vault secrets |

### 📬 Forms & Lead Capture
| Tool | Description |
|------|-------------|
| `configure_form` | Define form fields and submit actions (leads, email, webhook) |
| `list_forms` | List all configured forms |
| `remove_form` | Remove a form configuration |

### 🔒 Visitor Authentication
| Tool | Description |
|------|-------------|
| `configure_visitor_auth` | Enable email-based visitor login (magic link or code) |
| `get_visitor_auth_config` | Get current visitor auth settings |

### 📈 Tracking & Analytics
| Tool | Description |
|------|-------------|
| `set_tracking_scripts` | Set GA, GTM, Meta Pixel, or custom tracking scripts |
| `get_tracking_scripts` | Get current tracking configuration |
| `remove_tracking_scripts` | Remove all tracking scripts |
| `get_analytics` | Get visitor analytics: pageviews, referrers, devices, UTM |

### 🖼️ Visual Image Editor
| Tool | Description |
|------|-------------|
| `create_edit_session` | Create a browser-based image editing session |
| `get_edit_session_changes` | Read back what the user changed in the editor |

### ⏰ Scheduled Tasks
| Tool | Description |
|------|-------------|
| `create_scheduled_task` | Create a cron-based scheduled task |
| `list_scheduled_tasks` | List scheduled tasks with status and next run time |
| `delete_scheduled_task` | Delete a scheduled task |

### 📋 Task Tracker
| Tool | Description |
|------|-------------|
| `list_tasks` | List all tracked tasks with completion percentage |
| `get_task` | Get task details and status |
| `get_task_history` | Get full history of a task (progress, notes, documents) |
| `create_task` | Create a new task |
| `add_task_history` | Append progress update or note to a task |
| `export_tasks` | Export all tasks as Markdown grouped by status |

---

## 🌍 Supported AI Platforms

| Platform | Connection | Auth |
|----------|-----------|------|
| **Claude.ai** | Native Connector | OAuth 2.1 (automatic) |
| **Claude Desktop** | via `mcp-remote` | OAuth 2.1 (browser) |
| **Mistral / Le Chat** | Native MCP connector | OAuth 2.1 (automatic) |
| **ChatGPT** | GPT Actions | OAuth 2.0 |
| **Cursor** | via `mcp-remote` | OAuth 2.1 (browser) |
| **Windsurf** | via `mcp-remote` | OAuth 2.1 (browser) |
| **Claude Code** | via `mcp-remote` | OAuth 2.1 (browser) |
| **GitHub Copilot** | MCP config | OAuth 2.1 (browser) |
| **JetBrains AI** | MCP config | OAuth 2.1 (browser) |
| **Grok** | MCP config | OAuth 2.1 (browser) |
| **Gemini** | MCP config | OAuth 2.1 (browser) |

---

## 📋 Technical Details

| | |
|---|---|
| **Server URL** | `https://mcp.websitepublisher.ai/` |
| **Transport** | Streamable HTTP (JSON-RPC over POST) |
| **Protocol** | MCP 2025-06-18 |
| **Auth Discovery** | `https://mcp.websitepublisher.ai/.well-known/oauth-protected-resource` |
| **Auth Server** | `https://auth.websitepublisher.ai/.well-known/oauth-authorization-server` |
| **Token Types** | `wps_` (session), `wpr_` (refresh), `wpc_` (client ID) |
| **PKCE** | Required (S256) |
| **Scopes** | `mcp:read`, `mcp:write`, `mcp:full` |

---

## 💰 Pricing

| Plan | Price | Projects | Pages | Custom Domain |
|------|-------|----------|-------|---------------|
| **Free** | €0/mo | 1 | 5 | ❌ |
| **Starter** | €9/mo | 3 | 25/project | ✅ |
| **Pro** | €19/mo | 10 | Unlimited | ✅ |
| **Agency** | €49/mo | 50 | Unlimited | ✅ |

---

## 📖 Documentation

- **Getting Started:** [websitepublisher.ai/docs](https://websitepublisher.ai/docs)
- **MCP Setup:** [websitepublisher.ai/docs/mcp](https://websitepublisher.ai/docs/mcp)
- **API Reference (PAPI):** [websitepublisher.ai/docs/papi](https://websitepublisher.ai/docs/papi)
- **API Reference (MAPI):** [websitepublisher.ai/docs/mapi](https://websitepublisher.ai/docs/mapi)
- **Dashboard:** [dashboard.websitepublisher.ai](https://dashboard.websitepublisher.ai)
- **Privacy Policy:** [websitepublisher.ai/privacy](https://websitepublisher.ai/privacy)
- **Terms of Service:** [websitepublisher.ai/terms](https://websitepublisher.ai/terms)

---

## 🔒 Security & Privacy

- OAuth 2.1 with PKCE on all flows
- Dynamic Client Registration (RFC 7591) — no pre-shared secrets
- Rate limiting on all endpoints
- Content scanning and fraud prevention
- No credentials stored in client — token-based sessions only
- **Privacy Policy:** [websitepublisher.ai/privacy](https://websitepublisher.ai/privacy)

---

## 🏗 This Repository

This is the **documentation and integration repository** for the WebsitePublisher.ai MCP server. The server itself is a hosted SaaS service — no installation or self-hosting required. Just connect your AI client to `https://mcp.websitepublisher.ai/` and start building.

---

## 📬 Support

- **Docs:** [websitepublisher.ai/docs](https://websitepublisher.ai/docs)
- **Email:** support@websitepublisher.ai
- **Website:** [websitepublisher.ai](https://websitepublisher.ai)

---

<p align="center">
  <strong>WebsitePublisher.ai</strong> — The easiest way to build websites with AI.
</p>
