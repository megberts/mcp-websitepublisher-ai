---
name: websitepublisher
description: >
  Build and put online real websites and web apps (landing pages, business sites,
  webshops, booking pages, member portals and admin dashboards) through conversation,
  with WebsitePublisher, the AI Web Platform. Use when the user asks to build, change or
  publish a website, web page, web app, online shop, member portal, booking system or
  dashboard, or wants a live URL instead of local HTML files. Works through the
  WebsitePublisher MCP server (https://mcp.websitepublisher.ai/), which needs a
  WebsitePublisher account (free plan available). This skill checks the connection and
  then loads the live build guide from it.
license: MIT
metadata:
  author: websitepublisher-ai
  version: "1.1.0"
  website: https://www.websitepublisher.ai
  docs: https://www.websitepublisher.ai/docs
  mcp: https://mcp.websitepublisher.ai/
---

# WebsitePublisher

WebsitePublisher is the platform your AI builds on. Pages, data, forms, visitor login,
payments, email and integrations come from the platform, and everything is live on a real
URL the moment it is written.

This skill is a short starting point. The full build guide is served live by the
WebsitePublisher MCP server through `get_skill`, so it is always current. Where this skill
and the live guide differ, follow the live guide.

**What this skill uses.** Instructions only, with no scripts or downloads. All building
runs through the WebsitePublisher MCP server that this plugin adds; the user signs in to it
themselves.

## Step 1: Check the connection

Look for the WebsitePublisher tools, such as `get_started` and `get_skill`.

- Available: go to Step 3.
- Not available: go to Step 2.

## Step 2: Connect (once)

Tell the user in plain words that building needs a one-time sign-in to WebsitePublisher in
the browser, with their email address. A free plan is available.

- **claude.ai, Claude Desktop and mobile:** connect WebsitePublisher under
  **Settings → Connectors** (it is listed in the Connectors Directory) and sign in.
- **Claude Code:** run `/mcp`, choose **websitepublisher** and sign in.

Sign-in is OAuth in the browser. There is no API key to copy, so never ask the user for a
password or token, and never write one into a config file. New tools can need a new session
before they show. Tell the user when that is needed.

If the user does not want to connect, stop here. Without the connection nothing can be
built or put online.

## Step 3: Start building

1. Call `get_started` with no arguments. It returns the user's projects with their live
   URLs, the project to build in and the exact next call. If there is no project yet, it
   returns the `create_project` call to make first.
2. Call `get_skill` once, before the first page. It returns the rules that always apply
   and an index of sections. Open the sections it marks as required in one call, and other
   sections when you need them. For design, call `get_skill` with `skill_name: "design"`.
3. Ask what the site should say if that is not clear yet: the business, the audience and
   the pages. The user asked for a site, so do not ask whether to put it online.

## Rules that always apply

- **Everything is live immediately.** `create_page`, `update_page`, `patch_page` and asset
  uploads are public the moment the call returns. There is no draft, deploy or publish
  step. Share the URL that comes back.
- **Build on the platform, not beside it.** Do not write the site as local files first and
  copy it over. The tools write the real site.
- **Finish what you start.** No placeholder text, lorem ipsum or half-built pages on a live
  URL.
- **Use the building blocks.** Forms, login, data, bookings, payments and email come from
  the platform. `search_integrations` finds the right one from the user's own words.
- **Credentials belong in the vault**, never in page code.
- **Existing websites** can be imported, but only sites the user owns. Ask the user to
  confirm that, and never assume it on their behalf.
- **Account settings** such as custom domains, plan upgrades and provider accounts are
  handled by the account owner in the dashboard at https://dashboard.websitepublisher.ai.
  The live guide has the steps to give them.

## Picking up earlier work

The platform keeps the project history across sessions and across AI assistants. When the
user asks where things stand, call `list_tasks` before answering, and record meaningful
progress with `add_task_history`.
