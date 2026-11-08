---
name: superlist
description: "Manage the user's Superlist tasks and lists through the Superlist MCP server: capture tasks and reminders, plan projects as lists, check what's due today, update, move, assign, complete and share. Use whenever the user mentions Superlist, their to-dos, lists, inbox, or what's on their plate."
---

# Superlist

Superlist is a collaborative task and list manager. The Superlist MCP server
(`https://app.superlist.com/mcp`) lets you read and change the signed-in user's real
tasks and lists. Prefer acting in Superlist over answering from memory: the user's
data lives there.

## Before you start

The Superlist MCP server must be connected and signed in. It is a remote Streamable
HTTP server with OAuth 2.0: the user signs in with their Superlist account in the
browser, and no API key is needed. If the tools are missing, tell the user to connect
the server (setup guide: https://help.superlist.com/en/articles/658028-superlist-mcp-server)
instead of guessing.

Call `whoami` once if you need the user's name, email or account details.

## Find before you change

Tasks and lists are addressed by UUID. Never invent one.

1. Locate the item with `search` (natural-language query), `today`, `inbox` or `get_lists`.
2. Read its current state with `get_task`, `get_list` or `fetch` (`fetch` takes an id from `search` results).
3. Only then update, move, complete or delete it.

If several items match, ask the user which one they mean.

## Views

- `today`: open tasks due today or earlier that are assigned to the user (or created by them and unassigned). Use it for "what's on my plate?"
- `inbox`: tasks in the user's inbox. New tasks land here when no list is given.
- `get_lists`: the user's lists.

Read-only tools cap their results and return a `truncated` flag. When it is `true`, fall back to `search`.

## Creating tasks

`add_task` can create a fully formed task in one call, so you don't need follow-up calls:

- `title` (required), `list_uuid` (optional, defaults to the inbox)
- `due_date`: `YYYY-mm-DD`, or `YYYY-mm-DD HH:mm:ss` for a time. Times are in the user's timezone.
- `priority`: `none`, `high`, `medium` or `low`
- `labels`: up to 10 label names. Call `get_labels` first and reuse existing labels instead of inventing new ones.
- `reminder`: the user's own reminder as `YYYY-mm-DD HH:mm:ss`. Needs a due date. A timed due date gets a reminder at that time by default.
- `repetition`: needs a due date in the same call or already on the task. Types: `daily`, `weekly`, `monthly`, `days_of_week`, `nth_weekday_of_month`. Weekdays are 1–7 with 1 = Monday.
- `content`: markdown body. Lines like `- [ ] Subtask` become real subtasks.

Repetition examples:

- Every weekday: `{"type": "days_of_week", "days_of_week": [1, 2, 3, 4, 5]}`
- Every two weeks on Friday: `{"type": "weekly", "every": 2, "day_of_week": 5}`
- Last day of every month: `{"type": "monthly", "every": 1, "day": 0}`
- Third Monday of every month: `{"type": "nth_weekday_of_month", "every": 1, "week_of_month": 3, "day_of_week": 1}`

## Creating lists and projects

To plan a project, create the list and its tasks in one call: `add_list` with `title`,
an optional `emoji`, and `content` as markdown task items:

```markdown
- [ ] Draft the brief
- [ ] Collect feedback
- [ ] Ship it
```

For tasks that each need their own due date, priority or repetition, use `add_task`
with the `tasks` array instead. To nest lists, create the parent first, then create
each child with `parent_list_uuid`.

## Changing things in bulk

When the user asks to change several tasks, use one call with the array form, not a loop:

| Tool | Array argument |
| --- | --- |
| `add_task` | `tasks` |
| `update_task` | `updates` |
| `complete_task`, `reopen_task`, `delete_task`, `restore_task`, `unassign_task` | `task_uuids` |
| `move_task` | `moves` |
| `assign_task` | `assignments` |
| `add_label`, `remove_label` | `labels` |

Each takes up to 50 entries, and entries can carry different values. "Push these three
to Friday" is one `update_task` call. Pass either the single-item argument or the
array, never both.

## Editing a task or list body

The `content` argument of `update_task` and `update_list` replaces the whole body.
To add something:

1. Read the current body with `get_task` or `get_list` (`contents_markdown`).
2. Send the existing content plus your additions.
3. Keep existing subtask and sublist references exactly as returned, for example
   `- [ ] [Subtask title](superlist:task/<uuid>)`. Items without a reference become new
   subtasks, and existing ones you leave out are unlinked.

Checkbox state in `content` is ignored. Use `complete_task` and `reopen_task` to change it.

## Notes vs. messages

- A task's **notes** are its body (`contents_markdown`).
- Its **messages** are the chat thread collaborators write in. Read them with
  `get_messages` (pass the newest `synced_at` you've seen as `since` when checking for
  replies) and reply with `add_message`. Replying notifies everyone following the task.

To find out who changed a task or when ("who moved this due date?"), use `get_task_history`.

## Sharing and assigning

- `share_list` / `unshare_list` take a `list_uuid` plus a `user_email` or `user_uuid`.
  The user's plan limits on shared lists and collaborators apply.
- `assign_task` / `unassign_task` assign tasks to people who can access the list.

Confirm with the user before sharing a list with someone new.

## Be careful with destructive actions

- Confirm before calling `delete_task` or `delete_list` unless the user explicitly asked
  for that deletion.
- If something was deleted by mistake, try `restore_task` or `restore_list`.

## Example requests

- "What's on my plate today?" → `today`
- "Add 'Pick up dry cleaning' to my Errands list." → `get_lists` or `search` to find the list, then `add_task`
- "Move all unfinished tasks from yesterday to today." → `today`, then one `update_task` call with `updates`
- "Turn these meeting notes into a project in Superlist." → `add_list` with markdown task items
- "Remind me to pay rent on the 1st of every month." → `add_task` with `due_date` and a monthly `repetition`
