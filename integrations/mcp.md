---
description: Connect an MCP client to your Bots.Business account with OAuth and distinguish account tools from the public
  documentation MCP.
---


# Connect an AI client to your Bots.Business account

Bots.Business MCP lets a compatible AI client use account tools through OAuth. Use this endpoint:

{% code title="Example · Example 1" overflow="wrap" %}
```text
https://appapi.tgbot.ai/mcp
```
{% endcode %}

This is the **account MCP**. It can expose account operations permitted by your connection. The public documentation MCP at `https://help.bots.business/~gitbook/mcp` serves the help content and does not grant access to your bots.

## Connect

1. Open your AI client's MCP or connector settings.
2. Add the account endpoint above as a remote HTTP MCP server.
3. Start the connection. Follow the Bots.Business OAuth login and authorization flow in the browser.
4. Review the requested scope: `read` permits read operations; `write` permits applicable changes. Select the access you intend to give where the client supports scope selection.
5. Ask the client to list your bots as a first read-only check and confirm that the expected account is connected.

The AI client receives OAuth tokens; it does not need your Bots.Business password pasted into a chat. Do not use the Telegram bot token as an OAuth credential.

## Working with account tools

Give the client [the relevant help pages](../guides/ai-assistants.md) as well, so it can use the BJS contracts and examples when working with your bots.

The server derives its tool catalogue from the supported Bots.Business API operations. Let the client discover the tools for the current server version instead of hardcoding a stale list.

When requesting a change, name the intended bot and command and explain the result you want. Read the existing command first, then review the proposed change before authorizing actions that affect a working bot. Running a bot command can have side effects even when its name sounds like a diagnostic.

Read-only access is sufficient for inspecting your account. Writing command code requires the applicable write scope and account access; an expired or insufficient token cannot be fixed by passing a different bot ID.

<details>
<summary>Client configuration details</summary>

## Client configuration details

The public discovery endpoints are:

{% code title="Client configuration details · Example 2" overflow="wrap" %}
```text
https://appapi.tgbot.ai/.well-known/oauth-protected-resource
https://appapi.tgbot.ai/.well-known/oauth-authorization-server
```
{% endcode %}

They advertise authorization-code OAuth, PKCE `S256`, refresh tokens, and the `read`/`write` scopes. The OAuth resource is the external base URL, **`https://appapi.tgbot.ai`**, without `/mcp`. Prefer discovery over manually copying authorization/token endpoint settings.

This server uses POST JSON-RPC over HTTP. Opening `/mcp` with an ordinary browser GET returns 405; that alone does not mean MCP is broken. Test connection and tool discovery with an MCP client.

</details>

## Troubleshooting

- **`wrong_resource`:** use the advertised resource base URL, not the MCP path.
- **Not authorized:** reconnect the correct account, check scopes and token expiry.
- **No write operations available:** check the scope granted and the client's tool discovery.
- **Only documentation is available:** check that you connected the account endpoint rather than the public help endpoint.
- **Client requires SSE only:** use one compatible with this HTTP transport.

For direct requests outside an MCP client, use the [Bots.Business API documentation](https://dev.bots.business/). For local command-file editing, see [VS Code](vscode.md).
