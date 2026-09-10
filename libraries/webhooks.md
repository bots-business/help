---
description: Generate a command-specific webhook URL, receive JSON in BJS, and validate the sender before acting.
---

# Receive an external webhook

Install `Webhooks` when an external service needs to call a BJS command. A webhook URL selects the bot, command and optional user context. It does not prove that a payload came from your intended provider.

## Generate a URL for a callback

Run this from a private setup command for the relevant user:

```javascript
const url = Libs.Webhooks.getUrlFor({
  command: "/receive-event",
  user_id: user.id
});
Api.sendMessage({ text: url });
```

`user_id` is the internal BB user ID. Omitting it creates a URL without this explicit user selection; do not assume current-user functions will work in that callback. Treat generated URLs as capability links and share them only with the intended service.

## Receive JSON

Create `/receive-event`:

```javascript
if (!options || options.method !== "POST" || typeof content !== "string") {
  return;
}
let event;
try {
  event = JSON.parse(content);
} catch (error) {
  return;
}
if (!event || typeof event.type !== "string") { return; }
// A demonstration response only; no privileged action is performed.
WebApp.render({ content: { received: true }, mime_type: "application/json" });
```

Send `{"type":"test"}` as the JSON body to the generated URL from your test client. The callback's raw body is `content`; request information is in `options`, including `headers`, `method`, `params`, `url` and `ip`.

Header names are normalized by the backend, for example `Hmac` and `X-Hub-Signature-256`. `options` existing is not an authentication check: requests arriving at this URL can supply arbitrary bodies.

## Match the sender's protocol

Before applying a payment, deployment or account change:

1. Verify the provider's signature over the required original body using a secret stored for that integration.
2. Validate the event type, object ID, amount/identity and your expected current state.
3. Make repeated delivery safe using an event or transaction ID and a duplicate-handling design.
4. Return the exact response required by the provider.

The default webhook response is a Bots.Business JSON response, not the plain `ok` some providers require. The current backend allows JSON rendering but rejects non-JSON rendering, including the old `text/plain` acknowledgement recipe. Use a verified intermediary when the provider requires a different response. Returning successfully from BJS alone does not establish that a provider accepted the delivery.

## Compatibility notes

Use `getUrlFor`. The older `getUrl()` throws a deprecation error. The checked public `getGlobalUrl` implementation has an `api_key`/`apiKey` mismatch; do not use that old recipe as a working replacement.

An HTTP redirect to a page is not proof that a payment completed. For a specific signed payment example, see [OxaPay](../integrations/oxapay.md). For sending a request from BJS, see [HTTP](../bjs/http.md).
