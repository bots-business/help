# MCP

## How to connect Bots.Business MCP to your AI client

Bots.Business supports MCP — Model Context Protocol.

MCP allows AI clients to connect to your Bots.Business account and use available tools through secure OAuth authorization.

For most users, the setup is simple: add the MCP server URL to your AI client and connect your Bots.Business account.

### MCP Server URL

Use this URL:

```
https://appapi.tgbot.ai/mcp
```

Add it as an MCP server URL in your AI client.

After that, the AI client should open the Bots.Business login page. Log in to your Bots.Business account and approve the connection.

Your Bots.Business password is not shared with the AI client. The connection uses OAuth authorization.

***

### Basic setup

1. Open MCP / Connectors settings in your AI client.
2. Add MCP server URL:

```
https://appapi.tgbot.ai/mcp
```

3. Click **Connect**.
4. You will be redirected to Bots.Business login.
5. Log in to your Bots.Business account.
6. After successful login, the AI client will receive access to Bots.Business MCP tools.

***

### ChatGPT Connector setup

If Bots.Business MCP is used as a ChatGPT Connector, the end user usually does not need to configure OAuth manually.

The connector administrator should use this MCP URL:

```
https://appapi.tgbot.ai/mcp
```

OAuth must be enabled.

The ChatGPT OAuth redirect URL is supported:

```
https://chatgpt.com/connector/oauth/*
```

After the connector is configured, the user only needs to click **Connect**, log in to Bots.Business, and start using the available tools.

***

### For generic MCP clients

Bots.Business MCP server URL:

```
https://appapi.tgbot.ai/mcp
```

OAuth discovery endpoints:

```
https://appapi.tgbot.ai/.well-known/oauth-protected-resource
https://appapi.tgbot.ai/.well-known/oauth-authorization-server
```

OAuth endpoints:

```
Authorization URL: https://appapi.tgbot.ai/oauth/authorize
Token URL: https://appapi.tgbot.ai/oauth/token
```

OAuth parameters:

```
Scopes: read write
PKCE: S256
resource: https://appapi.tgbot.ai
Client ID: bots-business-mcp
```

Important: `resource` must be the external base URL without `/mcp`.

Correct:

```
resource=https://appapi.tgbot.ai
```

Incorrect:

```
resource=https://appapi.tgbot.ai/mcp
```

***

### Transport

Bots.Business MCP v1 supports stateless Streamable HTTP JSON-RPC.

Use:

```
POST /mcp
```

Example request:

```
POST https://appapi.tgbot.ai/mcp
Authorization: Bearer <access_token>
Content-Type: application/json
```

`GET /mcp` is not supported and will return:

```
405 Method Not Allowed
```

This is expected behavior. MCP clients should use `POST /mcp`.

SSE is not used in v1.

***

### Manual checks

Check OAuth discovery:

```
curl https://appapi.tgbot.ai/.well-known/oauth-protected-resource
```

Check MCP initialize:

```
curl -X POST https://appapi.tgbot.ai/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18"}}'
```

To call tools, use an OAuth access token:

```
curl -X POST https://appapi.tgbot.ai/mcp \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

***

### Troubleshooting

#### GET /mcp returns 405

This is normal.

Bots.Business MCP works through `POST /mcp`.

Use:

```
POST https://appapi.tgbot.ai/mcp
```

Do not use:

```
GET https://appapi.tgbot.ai/mcp
```

***

#### wrong\_resource error

If the token is rejected with `wrong_resource`, the OAuth `resource` does not match the external base URL.

The correct resource is:

```
https://appapi.tgbot.ai
```

It must not include `/mcp`.

If the application is running behind a proxy, make sure the backend sees the correct external protocol and host.

For example, if the backend thinks its base URL is:

```
http://localhost
```

or another internal host, the MCP token may be rejected as `wrong_resource`.

Check your proxy headers and external `base_url` configuration.

***

### Summary

For most users, only one URL is needed:

```
https://appapi.tgbot.ai/mcp
```

Add it to your AI client, click **Connect**, log in to Bots.Business, and the MCP tools will become available
