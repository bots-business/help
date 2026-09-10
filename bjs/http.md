---
description: Make BJS HTTP GET and POST requests, process success and error commands, parse JSON safely, and understand supported methods and redirects.
---

# HTTP requests

Use the uppercase `HTTP` object to call an external web service. The result is processed by a separate callback command. `HTTP.get(...)` and `HTTP.post(...)` do not return a downloaded response to the next line.

## A complete GET flow

Create `/check-service`:

```javascript
HTTP.get({
  url: "https://example.com/",
  success: "/service-response",
  error: "/service-error"
});
```

Create `/service-response`:

```javascript
if (typeof http_status === "undefined") { return; }
var status = Number(http_status);
if (status < 200 || status >= 300) {
  Bot.sendMessage("The service returned HTTP " + status + ".");
  return;
}
Bot.sendMessage("The service responded. Body length: " + String(content || "").length);
```

Create `/service-error`:

```javascript
Bot.sendMessage("The service could not be reached. Please try again later.");
```

The error callback is for request/transport failures. A completed response with an HTTP error status can still reach `success`; always check `http_status` there. The HTTP error callback does not promise an `options.error` payload like `Api.on_error`.

## Parameters

| Parameter | Meaning |
| --- | --- |
| `url` | Required absolute `http://` or `https://` URL |
| `success` | Name of the command that processes the response |
| `error` | Name of the command for a failed request; without it, the error is logged |
| `headers` | Request-header object |
| `body` | Request body for POST/PUT/DELETE/OPTIONS; objects are encoded as JSON |
| `cookies` | Cookie-header string when the remote service requires it |
| `folow_redirects` | Follow supported redirect responses; preserve this exact parameter spelling |

Implemented methods are `HTTP.get`, `HTTP.post`, `HTTP.put`, `HTTP.delete`, and `HTTP.options`. `HTTP.patch` and `HTTP.trace` are empty placeholders in the current implementation; do not use them as working requests. `Http.post`, `fetch`, and a promise-based `await HTTP.get(...)` are not replacements.

## POST JSON

Replace the example URL with an endpoint you control before running this command:

```javascript
// Command: /send-order
HTTP.post({
  url: "https://example.com/api/orders",
  headers: { "Content-Type": "application/json" },
  body: { product: "demo", quantity: 1 },
  success: "/order-response",
  error: "/service-error"
});
```

Create `/order-response`:

```javascript
if (Number(http_status) < 200 || Number(http_status) >= 300) {
  Bot.sendMessage("Order service returned HTTP " + http_status + ".");
  return;
}
var data;
try {
  data = JSON.parse(content);
} catch (error) {
  Bot.sendMessage("The service returned an unexpected response.");
  return;
}
Bot.sendMessage("Order reference: " + String(data.id || "not supplied"), { parse_mode: null });
```

`content` is already decoded text. Use `JSON.parse` only when JSON is expected. `http_headers` contains decoded response headers; `cookies` is the decoded Set-Cookie information when present. The HTTP client defaults to a JSON content type, but you can override the header and provide a correctly encoded string body for another format.

## Timing, redirects, and retries

Each HTTP network request has a fixed **5-second timeout** in the current implementation, including each request made while following redirects. There is no supported BJS `timeout` parameter to increase it. Requests also consume the separate overall command execution budget; several short requests can exhaust that budget.

A `background` option does not remove these limits. For a long external operation, use a service that promptly returns a job ID, save that ID, and [schedule a later status check](background.md). Handle the initial response and later polling through callback commands; do not hold one HTTP connection open for the entire job.

Prefer the final URL of the service. Automatic redirects repeat the original method, body and headers, including on 302/303 responses; they do not perform a browser-style POST-to-GET conversion. A redirected POST can therefore fail at a service that expects GET at its result URL. Relative locations can also fail. Do not follow untrusted redirects while sending credentials. Avoid automatically retrying a payment or creation request unless the external service provides an idempotency mechanism.

## HTTPS transport limitation

The current HTTP transport does not validate HTTPS server certificates. An `https://` URL alone therefore does not establish that the response came from the intended service. Do not treat direct HTTP examples as a production-ready way to transmit sensitive API keys, payment instructions or private records. A sensitive integration requires a transport with certificate validation and any required application-level authentication; a plain relay reached through the same unverified connection does not remove this limitation.

Do not accept an arbitrary URL or authorization header from a user. Keep service destinations under your control and [protect credentials](security.md). If a request fails, check the final URL, method, body encoding, response status, and bot error log before adding retries.
