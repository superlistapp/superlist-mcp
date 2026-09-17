# Superlist for Grok Build

Connect Grok Build to your Superlist account to manage tasks and lists in plain language. This plugin connects to the hosted Superlist MCP server using Streamable HTTP and OAuth.

## Requirements

- An active Superlist account.
- Grok Build with remote HTTP MCP and OAuth support.
- An internet connection. Your normal Superlist plan limits apply.

## Installation and sign-in

After the Superlist plugin is accepted into the xAI marketplace, open `/plugin` in Grok Build, find **Superlist**, and install it.

Connect the Superlist MCP server through Grok Build's MCP authentication flow. Sign in to Superlist in your browser and approve the requested access. The server address is `https://app.superlist.com/mcp`; no API key or credentials belong in the plugin files.

Marketplace availability and an end-to-end Grok Build sign-in test are pending for the initial submission. The configuration and public OAuth discovery endpoints have been checked separately.

## Example prompts

- "Show me what's due today in Superlist."
- "Add 'Review the launch plan' to my Work list in Superlist."
- "Find my Superlist tasks about the website launch."
- "Mark 'Review the launch plan' complete in Superlist."
- "Create a Superlist project plan from these meeting notes."

## Available capabilities

| Capability | Tools |
| --- | --- |
| Tasks | `add_task`, `get_task`, `update_task`, `complete_task`, `delete_task`, `move_task`, `assign_task`, `unassign_task` |
| Labels | `add_label`, `remove_label` |
| Lists | `add_list`, `get_list`, `get_lists`, `update_list`, `delete_list`, `share_list`, `unshare_list` |
| Views | `today`, `inbox` |
| Search and retrieval | `search`, `fetch` |
| Account identity | `whoami` |

These capabilities are provided by the hosted MCP server and may evolve independently of the plugin package. The plugin bundles one MCP connection and no skills, agents, commands, lifecycle hooks, or executable scripts.

## Authentication, permissions, and network access

Superlist uses OAuth authorization-code sign-in with support for PKCE (`S256`) and refresh tokens. Grok handles the user's authorization and connection credentials. This plugin does not embed secrets, read local credential files or environment variables, run shell commands, or add telemetry.

The connected assistant can access Superlist data available to the signed-in account. Its tools can read, create, update, complete, and delete tasks and lists, and change list sharing. Connect only the account you intend to use and review requests that change or share your data. The plugin does not request a separate filesystem or shell permission.

The MCP connection and OAuth discovery use these Superlist endpoints:

| Endpoint | Purpose |
| --- | --- |
| `https://app.superlist.com/mcp` | MCP requests and tool operations |
| `https://app.superlist.com/.well-known/oauth-protected-resource` | Discover the protected resource and authorization server |
| `https://app.superlist.com/.well-known/oauth-authorization-server` | Discover OAuth endpoints and supported authentication methods |
| `https://app.superlist.com/oauth/register` | Register an OAuth client when required |
| `https://app.superlist.com/oauth/authorize` | Browser sign-in and consent |
| `https://app.superlist.com/oauth/token` | Obtain or refresh access tokens |

The browser sign-in experience may involve the identity provider you choose. The plugin itself configures only the Superlist MCP endpoint. Remove or disconnect the Superlist connection in your client when you no longer want it to access your account.

## Verification before release

In Grok Build, complete sign-in, read your Today view, create and update a disposable test task, complete it, and verify reconnecting works. Catalog validation alone does not verify this authenticated user flow.

## Help and policies

- [Superlist MCP overview](https://www.superlist.com/mcp)
- [Setup and troubleshooting](https://help.superlist.com/en/articles/658028-superlist-mcp-server)
- [Privacy policy](https://www.superlist.com/privacy-policy)
- [Terms](https://www.superlist.com/terms)
- For support, open Superlist and choose **Help → Contact support**.

## License

The plugin files in this `grok/` directory are licensed under the [MIT License](LICENSE). This license covers this plugin package only; it does not license the hosted Superlist service or other repository content. Use of the service remains subject to Superlist's terms.
