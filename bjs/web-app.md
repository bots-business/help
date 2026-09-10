---
description: Return JSON from BJS with WebApp, generate endpoint URLs, understand the current HTML restriction, and validate
  public request data.
---


# Web App

`WebApp` lets a BJS command return a web response. **The current public renderer serves JSON; non-JSON HTML, CSS, JavaScript, and plain-text responses are disabled and return 404.** Older examples showing a hosted HTML page no longer describe the current behavior.

This is separate from the Bots.Business app interface. A Telegram Mini App interface needs a suitable external HTTPS host while HTML rendering remains disabled here. Do not change a MIME type to disguise HTML as JSON.

For a complete HTML form, launch button, and BJS submission handler, follow [Build a Telegram Mini App form](../guides/mini-app.md).

## Return a JSON response

Create a command named `web-home` with this BJS:

{% code title="Return a JSON response · Example 1" overflow="wrap" %}
```javascript
WebApp.render({
  content: JSON.stringify({ message: "Hello!", status: "ok" }),
  mime_type: "application/json"
});
```
{% endcode %}

Create `/open-web`:

{% code title="/open-web" overflow="wrap" %}
```javascript
var url = WebApp.getUrl({ command: "web-home" });
Bot.sendMessage(url, { parse_mode: null });
```
{% endcode %}

Send `/open-web` to the bot and open the link. The response should be a JSON object containing `message` and `status`. The rendering command must contain `WebApp.render`, the bot must be running, and its account must have available quota.

## Methods and parameters

| Method | Parameters |
| --- | --- |
| `WebApp.getUrl({command, options})` | Generate a URL for a rendering command; `options` adds query data |
| `WebApp.render({content, mime_type})` | Supply response content; use valid JSON and `application/json` for the current public renderer |
| `WebApp.render({template, options, mime_type})` | Read and evaluate a named command's template; the resulting response still passes the same JSON-only renderer |

Use the URL generator instead of hard-coding an API host: the generated URL uses the bot's configured API gateway. `getUrl` does not publish an arbitrary function; its `command` must exist and be suitable for rendering.

## Query data and JSON responses

A web request's data is available in `options`. A GET request uses query parameters; a POST uses body parameters. Validate every value before using it.

Create `web-status`:

{% code title="web-status" overflow="wrap" %}
```javascript
var topic = options && typeof options.topic === "string" ? options.topic : "general";
WebApp.render({
  content: JSON.stringify({ status: "ok", topic: topic.slice(0, 50) }),
  mime_type: "application/json"
});
```
{% endcode %}

Generate its URL in a chat command:

{% code title="Query data and JSON responses · Example 4" overflow="wrap" %}
```javascript
Bot.sendMessage(WebApp.getUrl({
  command: "web-status",
  options: { topic: "help" }
}), { parse_mode: null });
```
{% endcode %}

Use small text values in URL options; keep private data and credentials out of URLs.

<details>
<summary>Templates and existing HTML projects</summary>

## Templates and existing HTML projects

`template` names another command whose code is used as template text. `<% ... %>` evaluates an expression and inserts its result; `options` supplies the template's values. This template mechanism does not bypass the public renderer's restrictions. An old `page.html` template can still be found and evaluated yet produce a 404 when the server refuses its HTML response.

For JSON responses, `JSON.stringify` is simpler and safer than hand-building JSON with template interpolation. Move an existing HTML interface to an appropriate external host if you need to keep using it, and validate the data flow separately. A payment provider that requires a literal plain-text acknowledgement such as `ok` cannot be assumed compatible with this JSON-only endpoint; confirm its documented acknowledgement contract before enabling live callbacks.

</details>

## Authentication and important changes

Treat this rendering endpoint as public. A generated URL contains a `secret` parameter, but the current rendering controller does not validate it as a signed authorization check. Possessing a link or passing `user_id` in `options` does not prove a Telegram user's identity.

Do not credit balances, approve payments, reset credentials, or grant roles based on web query data. For a Mini App, validate Telegram's signed `initData` on a trusted server and apply your own permission and replay checks before making important changes. `initDataUnsafe` is not verified identity. [Telegram Mini App validation](https://core.telegram.org/bots/webapps#validating-data-received-via-the-mini-app).

A separate webhook also needs [a checked secret and appropriate event validation](../libraries/webhooks.md#match-the-senders-protocol); changing the route's name does not make a request trustworthy.

## Troubleshooting

For “Command not found” or “No content,” verify the exact command name, `WebApp.render` in its code, running bot status, quota, and template name. A 404 saying HTML is temporarily disabled is the server's rendering restriction, not a missing template or incorrect URL. A web trigger may have no Telegram chat/user, so a command that only calls `Bot.sendMessage` does not return a web response. See [context](context.md) and [security](security.md).
