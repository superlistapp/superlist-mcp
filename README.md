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

Tasks: add_task, get_task, update_task, complete_task, delete_task, move_task, assign_task, unassign_task

Labels: add_label, remove_label

Lists: add_list, get_list, get_lists, update_list, delete_list, share_list, unshare_list

Views: today (tasks due today or earlier assigned to you), inbox (tasks from your inbox)

Search & retrieval: search (search your tasks and lists), fetch (get full content of a task or list by id)

Identity: whoami (info about the authenticated Superlist user)

Connecting to Claude


Open Claude and go to Settings → Connectors → Add Connector.
Paste in the Superlist MCP URL: https://app.superlist.com/mcp
Click Connect and sign in with your Superlist account when prompted.
Approve the requested permissions.


Any AI tool that supports custom MCP connectors will work — look for a "Connectors," "Integrations," or "MCP server" option in its settings. See the setup guide for ChatGPT and other tools.

Security

The connection uses standard OAuth. You sign in directly to Superlist — the AI tool never sees your password, it only accesses data your account already has access to, and you can revoke access at any time by removing the connector in your AI tool.

Links


Website: https://www.superlist.com
MCP overview: https://www.superlist.com/mcp
Help center: https://help.superlist.com/en/articles/658028-superlist-mcp-server
Registry name: io.github.superlistapp/superlist
