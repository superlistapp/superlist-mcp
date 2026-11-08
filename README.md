Superlist MCP Server

The task layer for your AI agents. Connect Superlist to Claude, ChatGPT, Cursor, or any MCP-compatible AI assistant and manage your tasks, lists, due dates, and routines in plain language — without leaving the chat.


Server URL: https://app.superlist.com/mcp
Transport: Remote (Streamable HTTP)
Authentication: OAuth 2.0 — sign in with your Superlist account
Homepage: https://www.superlist.com/mcp
Setup guide: https://help.superlist.com/en/articles/658028-superlist-mcp-server


What it does

The Superlist MCP server is a secure bridge between your Superlist account and any AI assistant that supports the Model Context Protocol (MCP). Once connected, your assistant can read and update your lists and tasks using everyday language — no copy-pasting, no switching tabs.

Things you can ask your assistant once it's connected:


"What's on my plate today?"
"Add 'Pick up dry cleaning' to my Errands list."
"Move all unfinished tasks from yesterday to today."
"Draft a project plan in Superlist based on this meeting transcript."
"Summarize what I got done this week."


Available tools

Tasks: add_task, get_task, update_task, complete_task, reopen_task, delete_task, restore_task, move_task, assign_task, unassign_task, get_task_history

Messages: get_messages, add_message

Labels: get_labels, add_label, remove_label

Lists: add_list, get_list, get_lists, update_list, delete_list, restore_list, share_list, unshare_list

Views: today (tasks due today or earlier assigned to you), inbox (tasks from your inbox)

Search & retrieval: search (search your tasks and lists), fetch (get full content of a task or list by id)

Identity: whoami (info about the authenticated Superlist user)

Connecting to Grok Build

The [Superlist plugin for Grok Build](grok/README.md) packages the hosted MCP connection for the xAI plugin marketplace. See its README for setup, permissions, and the initial submission status. The plugin files live in `grok/` and have their own license.

Connecting to Claude


Open Claude and go to Settings → Connectors → Add Connector.
Paste in the Superlist MCP URL: https://app.superlist.com/mcp
Click Connect and sign in with your Superlist account when prompted.
Approve the requested permissions.


Any AI tool that supports custom MCP connectors will work — look for a "Connectors," "Integrations," or "MCP server" option in its settings. See the setup guide for ChatGPT and other tools.

Agent skill

This repository includes a Superlist agent skill (skills/superlist/SKILL.md). It teaches your agent how to use the Superlist tools well: finding items before changing them, creating tasks with due dates and repetition in one call, changing many tasks at once, and confirming before deleting. Install it into Claude Code, Cursor, Codex and other agents that support skills:

```bash
npx skills add superlistapp/superlist-mcp
```

The skill needs the Superlist MCP server connected in the same agent.

Connecting to Hermes Agent

This repository is also a portable Agent Plugins package (plugin.json + mcp.json + skills/) for Hermes Agent. Hermes needs MCP support; with a pip install that is the mcp extra (pip install "hermes-agent[mcp]").

```bash
hermes plugins install superlist
hermes plugins enable superlist
```

(Before the catalog listing is live: `hermes plugins install superlistapp/superlist-mcp`)

Then add the server with OAuth to ~/.hermes/config.yaml:

```yaml
mcp_servers:
  superlist:
    url: "https://app.superlist.com/mcp"
    auth: oauth
```

Sign in with your Superlist account and check the connection:

```bash
hermes mcp login superlist
hermes mcp test superlist
```

On a remote machine, follow the prompt: paste the redirect URL back, or forward the port it shows.

What the Hermes plugin does: it contains configuration and one skill only, with no local code. Every tool call goes to app.superlist.com with the signed-in user's permissions. Tools can create, update, complete, delete and restore tasks and lists, and change list sharing; the skill tells the agent to confirm before deleting or sharing, but Hermes does not block those calls. Sign-in tokens are stored by Hermes, not by this plugin. No telemetry. The tool list is served by the remote server and can change without a new plugin version.

Security

The connection uses standard OAuth. You sign in directly to Superlist — the AI tool never sees your password, it only accesses data your account already has access to, and you can revoke access at any time by removing the connector in your AI tool.

Links


Website: https://www.superlist.com
MCP overview: https://www.superlist.com/mcp
Help center: https://help.superlist.com/en/articles/658028-superlist-mcp-server
Registry name: io.github.superlistapp/superlist
