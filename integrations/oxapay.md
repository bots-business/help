---
description: Create an OxaPay sandbox invoice from BJS, process its response, and verify signed payment callbacks.
---


# Create an OxaPay test invoice

Use `Libs.OxaPayLibV1` to call OxaPay's `/v1` API. Start with a **sandbox invoice**, then validate callback handling before enabling live payments. The older `OxaPayLib` is a different library and should not be substituted into this example.

The library uses the current BJS HTTP transport, which does not validate HTTPS certificates. Use a disposable test account/key for this example; the invoice's sandbox flag does not protect the key in transit. Production use requires resolving the [HTTP transport limitation](../bjs/http.md#https-transport-limitation) as well as the acknowledgement limitation below.

{% stepper %}

{% step %}

### Prepare the bot

1. Install `OxaPayLibV1` and `Webhooks` for a test bot.
2. Obtain a merchant API key from your OxaPay account.
3. Store it through `Libs.OxaPayLibV1.setMerchantApiKey("YOUR_MERCHANT_API_KEY")` in an owner-only setup command. Remove the literal from code afterward; do not expose setup to ordinary users or commit keys to a public repository.
4. Create the three commands below. Test with sandbox data before using a live invoice.

The merchant key handles payment endpoints. `setPayoutApiKey` and `setGeneralApiKey` configure the separate payout and general API families; they are not needed for this invoice example.

{% endstep %}

{% step %}

### Request an invoice <a href="#1-request-an-invoice" id="1-request-an-invoice"></a>

In `/test-invoice`:

{% code title="1. Request an invoice · Example 1" overflow="wrap" %}
```javascript
const orderId = "test-" + user.id + "-" + Date.now();
User.setProp("oxapayTestOrder", orderId);
Libs.OxaPayLibV1.apiCall({
  url: "/payment/invoice",
  method: "post",
  fields: {
    amount: 1,
    currency: "USD",
    order_id: orderId,
    description: "Test order",
    sandbox: true,
    on_callback: "/test-payment-update"
  },
  on_success: "/test-invoice-created"
});
```
{% endcode %}

Use lowercase **`"post"`**. The checked library sends POST only for that exact value; `"POST"` goes through its GET branch. Omitting `method` also selects `post`.

`on_callback` is the library's convenience field: it generates a Webhooks URL with the current internal BB user ID. `on_success` names the BJS command receiving the API response. OxaPay's invoice endpoint returns a payment URL and track ID; its sandbox flag requests test mode. [Invoice API](https://docs.oxapay.com/api-reference/payment/generate-invoice).

{% endstep %}

{% step %}

### Handle the API response <a href="#2-handle-the-api-response" id="2-handle-the-api-response"></a>

In `/test-invoice-created`:

{% code title="2. Handle the API response · Example 2" overflow="wrap" %}
```javascript
if (!options || options.status !== 200 || !options.data) {
  Bot.sendMessage("The test invoice could not be created.");
  return;
}
if (!options.data.payment_url || !options.data.track_id) {
  Bot.sendMessage("The invoice response was incomplete.");
  return;
}
User.setProp("oxapayTestTrackId", String(options.data.track_id));
Api.sendMessage({ text: "Test invoice:\n" + options.data.payment_url });
```
{% endcode %}

The library parses JSON and passes it in `options`, not in `options.body`. A transport error raises a library error; `on_success` is not a guarantee that an invoice was accepted, which is why this command checks the response status and data.

{% endstep %}

{% step %}

### Handle a payment update <a href="#3-handle-a-payment-update" id="3-handle-a-payment-update"></a>

The library's webhook handler checks an HMAC-SHA512 signature over the raw callback body before dispatching to your command. It selects the merchant secret for invoice/white-label/static-address callbacks and the payout secret otherwise. OxaPay requires signed callbacks and an acknowledgement response. [Webhook contract](https://docs.oxapay.com/webhook).

In `/test-payment-update`:

{% code title="3. Handle a payment update · Example 3" overflow="wrap" %}
```javascript
if (!options || options.type !== "invoice") { return; }
const expectedOrder = User.getProp("oxapayTestOrder");
if (options.order_id !== expectedOrder) { return; }
const expectedTrack = User.getProp("oxapayTestTrackId");
if (expectedTrack && String(options.track_id) !== expectedTrack) { return; }

const status = String(options.status || "").toLowerCase();
if (status === "paid") {
  User.setProp("oxapayTestStatus", "paid");
} else if (status === "paying") {
  User.setProp("oxapayTestStatus", "paying");
}
WebApp.render({ content: { received: true }, mime_type: "application/json" });
```
{% endcode %}

This demonstrates recording test status only. It does not deliver goods or credit a balance. Read `User.getProp("oxapayTestStatus")` from a separate test-status command. A new test invoice replaces these demo properties; a real application needs a record per order.

**Direct callback acknowledgement needs additional integration work.** OxaPay requests a plain `ok` acknowledgement, but the current Bots.Business rendering path allows JSON and rejects the old `text/plain` recipe. The JSON response above does not claim to satisfy OxaPay's acknowledgement protocol; retries can occur. For a live integration, use a verified intermediary that authenticates the provider event, forwards it through a separately authenticated bot entry, and returns the required acknowledgement, or first confirm an explicitly supported acknowledgement path with Bots.Business. Do not enable automatic fulfillment until the complete delivery protocol is tested.

Before live use, verify in your provider's delivery logs that a signed test callback reaches the right user context and returns the expected acknowledgement. Test invalid signatures, repeated callbacks and a callback arriving before the invoice-created response. For fulfillment, additionally verify the stored order, expected amount/currency, provider track ID and payment state, with durable duplicate handling. A return URL or an unverified command invocation is not payment evidence.

For guarded white-label, payout and swap examples, endpoint/key mappings, and explicit status lookup, see [OxaPay operations](oxapay-operations.md).

{% endstep %}

{% endstepper %}

## Other methods and troubleshooting

`apiCall({ url, method, fields, on_success })` supports this wrapper's GET/POST paths. Use current provider endpoint documentation; do not infer support for PATCH/DELETE from the generic name. The library chooses an API key from `payment`, `payout` or `general` in the path.

- Missing merchant key: configure it in the bot that is making the request.
- Wrong method or unexpected response: check lowercase `post`, endpoint path and API generation.
- Invalid HMAC: check the matching key, raw body and the `Hmac` header reaching the bot.
- No payment update: check Webhooks installation, callback command, selected user and provider delivery response.
- Payout and swap examples can move real funds; implement them as separately authorized flows, not as user-callable copies of a setup recipe.

For lower-level request handling, see [HTTP](../bjs/http.md) and [Webhooks](../libraries/webhooks.md).
