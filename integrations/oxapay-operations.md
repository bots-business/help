---
description: Use OxaPayLibV1 for white-label payment details, payouts and swaps, with restricted request previews, response checks and status lookup.
---

# OxaPay white-label payments, payouts and swaps

`Libs.OxaPayLibV1` supports these operations as well as the [sandbox invoice example](oxapay.md). A white-label payment gives you an address and payment details to display in your bot. A payout sends funds from your account; a swap exchanges account assets.

The examples below start in **preview mode** and send no request until you explicitly enable one. Preview mode is local example code, not an OxaPay sandbox. The documented white-label, payout and swap request schemas do not provide the invoice's `sandbox` flag. The current [HTTP certificate-validation limitation](../bjs/http.md#https-transport-limitation) still applies to this library. Resolve it before sending real keys or trusting payment responses in production.

## Endpoint, key and response reference

Pass paths relative to `https://api.oxapay.com/v1` in `apiCall`. The three key setters configure different API families.

| Operation and path | Method / key setter | Successful `options.data` contains |
| --- | --- | --- |
| [Create white-label payment](https://docs.oxapay.com/api-reference/payment/generate-white-label): `/payment/white-label` | `post` / `setMerchantApiKey` | `track_id`, `address`, `pay_amount`, `pay_currency`, `network`, `memo`, `expired_at`, `qr_code` |
| [Read a payment](https://docs.oxapay.com/api-reference/payment/payment-information): `/payment/{track_id}` | `get` / `setMerchantApiKey` | Payment details and its current `status` |
| [Create payout](https://docs.oxapay.com/api-reference/payout/generate-payout): `/payout` | `post` / `setPayoutApiKey` | `track_id`, `status`; creation is not proof of completed delivery |
| [Read a payout](https://docs.oxapay.com/api-reference/payout/payout-information): `/payout/{track_id}` | `get` / `setPayoutApiKey` | `track_id`, `status`, `address`, `currency`, `amount`, network and transaction details |
| [Available swap pairs](https://docs.oxapay.com/api-reference/swap/swap-pairs): `/general/swap/pairs` | `get` / `setGeneralApiKey` | `list` of pairs with their `min_amount` |
| [Calculate swap](https://docs.oxapay.com/api-reference/swap/swap-calculate): `/general/swap/calculate` | `post` / `setGeneralApiKey` | `amount`, `to_amount`, `rate`; this does not execute a swap |
| [Execute swap](https://docs.oxapay.com/api-reference/swap/swap-request): `/general/swap` | `post` / `setGeneralApiKey` | `track_id`, `from_currency`, `to_currency`, `from_amount`, `to_amount`, `rate`, `date` |

Use lowercase `"post"` or `"get"`. The checked wrapper sends POST only for the exact value `"post"`; uppercase `"POST"` selects its GET branch. It parses the JSON response and passes the whole object in `options` to `on_success`. Check numeric `options.status === 200` and the operation's required data. An API error can reach this callback too; a transport error raises a library error. Swap success has no documented `data.status` field.

## Prepare a restricted example

1. Install `OxaPayLibV1` in a disposable bot. These examples use API responses and explicit status lookup; they do not generate webhook URLs.
2. Obtain your own Telegram user ID using the [trusted-user setup](../bjs/security.md#restrict-a-command-to-a-trusted-telegram-user). Replace `YOUR_TELEGRAM_USER_ID` in **every** command below. Keep **Answer** and **Keyboard** empty and **Wait for answer** off. Use a private conversation with the bot.
3. Previewing needs no key. For an authorized request after resolving the transport limitation, copy the protected `/owner-status` command from the trusted-user setup. Keep its guard and replace its final confirmation message with only the required setter: `Libs.OxaPayLibV1.setMerchantApiKey("YOUR_KEY")`, `Libs.OxaPayLibV1.setPayoutApiKey("YOUR_KEY")`, or `Libs.OxaPayLibV1.setGeneralApiKey("YOUR_KEY")`. Replace the key placeholder, save and run this temporary setup from your configured account, then remove the key literal and setup command. Never take keys or the administrator ID from another visitor's message.
4. Create `/oxa-operation`, `/oxa-result`, `/oxa-check` and `/oxa-checked` with the BJS below. Leave `SEND_REQUEST = false` initially.

The fixed values are example requests, not a statement about currently accepted amounts or networks. Before authorizing a request, check the provider's supported currencies/networks, your account permissions and balance, and the minimum for the selected swap pair. Set the recipient yourself and include a memo/tag when its network requires one.

## Preview and request an operation

Create `/oxa-operation`:

```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID ||
    !chat || String(chat.chatid) !== ADMIN_TELEGRAM_ID) { return; }

const SEND_REQUEST = false;
const ATTEMPT_ID = "REPLACE_WITH_UNIQUE_ATTEMPT_ID";
const input = /^(white-label|payout|swap)(?: (CONFIRM))?$/.exec(String(params || "").trim());
if (!input) {
  Bot.sendMessage("Use /oxa-operation white-label, payout or swap.");
  return;
}
const kind = input[1];
let path;
let fields;
if (kind === "white-label") {
  path = "/payment/white-label";
  fields = {
    amount: 10, currency: "USD", pay_currency: "TRX", network: "Tron",
    lifetime: 60, order_id: "bb-" + ATTEMPT_ID
  };
} else if (kind === "payout") {
  path = "/payout";
  fields = {
    address: "YOUR_VERIFIED_RECIPIENT_ADDRESS",
    amount: 10, currency: "TRX", network: "Tron",
    description: "BB example " + ATTEMPT_ID
  };
} else {
  path = "/general/swap";
  fields = { from_currency: "BTC", to_currency: "USDT", amount: 0.0001 };
}
if (!SEND_REQUEST || input[2] !== "CONFIRM") {
  Api.sendMessage({
    text: "Preview only. No request sent.\n" + path + "\n" + JSON.stringify(fields),
    parse_mode: null
  });
  return;
}
if (ATTEMPT_ID === "REPLACE_WITH_UNIQUE_ATTEMPT_ID" ||
    !/^[A-Za-z0-9_-]{1,60}$/.test(ATTEMPT_ID) ||
    (kind === "payout" && fields.address === "YOUR_VERIFIED_RECIPIENT_ADDRESS")) {
  Bot.sendMessage("Configure a unique attempt ID and the verified recipient in the editor.");
  return;
}
if (!Number.isFinite(fields.amount) || fields.amount <= 0) {
  Bot.sendMessage("Configure a positive finite amount.");
  return;
}
const property = "oxaDemo:" + ATTEMPT_ID;
if (User.getProp(property)) {
  Bot.sendMessage("This attempt is already recorded. Check its provider status before any new request.");
  return;
}
User.setProp(property, { kind: kind, fields: fields, state: "attempted" }, "json");
Libs.OxaPayLibV1.apiCall({
  url: path,
  method: "post",
  fields: fields,
  on_success: "/oxa-result " + ATTEMPT_ID
});
```

Send `/oxa-operation white-label`, `/oxa-operation payout`, and `/oxa-operation swap` separately. Each should print its path and fields with “Preview only.” Test from a different Telegram account too: it should receive nothing.

For a deliberately authorized request, configure the fields and a fresh attempt ID in the editor, enable `SEND_REQUEST`, then send, for example, `/oxa-operation payout CONFIRM`. This sends a **real payout request**. Return `SEND_REQUEST` to `false` after the attempt. The same steps apply to `white-label` and `swap`.

The stored attempt blocks an ordinary sequential repeat; it is **not an atomic lock or provider idempotency key**. Use this as a single-operator example. Automated or concurrent withdrawals require a durable authorization and duplicate-prevention design. After a timeout or unknown result, inspect the provider account/history before deciding what happened; never delete the attempt and resend just to obtain a response.

## Handle each response correctly

Create `/oxa-result`. `params` is the attempt ID appended to `on_success`; `options` is the parsed API response.

```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const attemptId = String(params || "").trim();
if (!/^[A-Za-z0-9_-]{1,60}$/.test(attemptId)) { return; }
const property = "oxaDemo:" + attemptId;
const saved = User.getProp(property);
if (!saved || saved.state !== "attempted") { return; }
const data = options && options.data;
if (!options || options.status !== 200 || !data ||
    typeof data.track_id !== "string" || !data.track_id) {
  Bot.sendMessage("No confirmed API result. Check the provider account before retrying.");
  return;
}
function positive(value) {
  return typeof value === "number" && Number.isFinite(value) && value > 0;
}
function sameCurrency(a, b) {
  return typeof a === "string" && a.toUpperCase() === b.toUpperCase();
}
let text;
if (saved.kind === "white-label") {
  if (data.order_id !== saved.fields.order_id ||
      data.amount !== saved.fields.amount || !sameCurrency(data.currency, saved.fields.currency) ||
      !positive(data.pay_amount) || !sameCurrency(data.pay_currency, saved.fields.pay_currency) ||
      typeof data.address !== "string" || !data.address ||
      typeof data.network !== "string" || !data.network ||
      !positive(data.expired_at) || data.expired_at * 1000 <= Date.now()) {
    Bot.sendMessage("Payment details are incomplete, expired or inconsistent. Inspect the provider record.");
    return;
  }
  text = "Payment details created; payment is not confirmed.\n" +
    "Amount: " + data.pay_amount + " " + data.pay_currency +
    "\nNetwork: " + data.network + "\nAddress: " + data.address +
    (data.memo ? "\nRequired memo/tag: " + data.memo : "") +
    "\nExpires: " + new Date(data.expired_at * 1000).toISOString();
} else if (saved.kind === "payout") {
  if (typeof data.status !== "string" || !data.status) {
    Bot.sendMessage("Payout response has no status. Inspect the provider record.");
    return;
  }
  text = "Payout request accepted. Provider status: " + data.status;
} else if (saved.kind === "swap") {
  if (!sameCurrency(data.from_currency, saved.fields.from_currency) ||
      !sameCurrency(data.to_currency, saved.fields.to_currency) ||
      data.from_amount !== saved.fields.amount ||
      !positive(data.to_amount) || !positive(data.rate)) {
    Bot.sendMessage("Swap result is incomplete or inconsistent. Inspect the provider history.");
    return;
  }
  text = "Swap result: " + data.from_amount + " " + data.from_currency +
    " → " + data.to_amount + " " + data.to_currency + "\nRate: " + data.rate;
} else { return; }
saved.state = "response_received";
saved.trackId = data.track_id;
saved.result = data;
User.setProp(property, saved, "json");
Api.sendMessage({
  text: text + "\nTrack ID: " + data.track_id + "\nAttempt: " + attemptId,
  parse_mode: null
});
```

For white-label payments, display the returned payment amount, currency, network, address, any memo/tag and expiry together. A QR code alone can omit information the payer needs. Do not reuse an expired address. For payouts, keep the track ID and check the eventual status; a successful creation response is not a blockchain receipt. For swaps, record the actual returned amounts/rate rather than a previous calculation. [White-label contract](https://docs.oxapay.com/api-reference/payment/generate-white-label), [payout contract](https://docs.oxapay.com/api-reference/payout/generate-payout), [swap contract](https://docs.oxapay.com/api-reference/swap/swap-request).

## Check a payment or payout

Create `/oxa-check` and send `/oxa-check YOUR_ATTEMPT_ID` after receiving its track ID:

```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID ||
    !chat || String(chat.chatid) !== ADMIN_TELEGRAM_ID) { return; }
const attemptId = String(params || "").trim();
if (!/^[A-Za-z0-9_-]{1,60}$/.test(attemptId)) { return; }
const saved = User.getProp("oxaDemo:" + attemptId);
if (!saved || !saved.trackId) {
  Bot.sendMessage("No track ID is saved. Inspect the provider account/history.");
  return;
}
if (saved.kind !== "white-label" && saved.kind !== "payout") {
  Bot.sendMessage("For a swap, compare the saved response with your OxaPay swap history.");
  return;
}
Libs.OxaPayLibV1.apiCall({
  url: (saved.kind === "payout" ? "/payout/" : "/payment/") + encodeURIComponent(saved.trackId),
  method: "get",
  on_success: "/oxa-checked " + attemptId
});
```

Create `/oxa-checked`:

```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const attemptId = String(params || "").trim();
if (!/^[A-Za-z0-9_-]{1,60}$/.test(attemptId)) { return; }
const saved = User.getProp("oxaDemo:" + attemptId);
const data = options && options.data;
if (!saved || !options || options.status !== 200 || !data ||
    String(data.track_id) !== saved.trackId ||
    typeof data.status !== "string" || !data.status) {
  Bot.sendMessage("Status lookup was not verified. Check the provider record.");
  return;
}
saved.lastReportedStatus = data.status;
User.setProp("oxaDemo:" + attemptId, saved, "json");
Api.sendMessage({ text: "Provider status for " + saved.trackId + ": " + data.status, parse_mode: null });
```

This reports a provider status; it does not credit a user, deliver goods or initiate another transfer. Before such actions, verify the stored order/recipient, expected amount and currency, final payment state and duplicate handling through a trusted integration.

## Callbacks and verification

For asynchronous payment or payout notifications, install `Webhooks` and set the library convenience field `on_callback` in a request's `fields`. The library generates a URL for the current internal BB user and verifies incoming HMAC-SHA512 using the merchant key for `white_label` or the payout key for payouts. Your callback receives the parsed event in `options`, with no outer API-response envelope. The webhook event type is `white_label`, although the request path uses `white-label`. Do not apply the response check `options.status === 200` to a webhook event. [Provider webhook contract](https://docs.oxapay.com/webhook).

Direct webhook acknowledgement has the [same unresolved limitation as the invoice example](oxapay.md#3-handle-a-payment-update). The code on this page uses explicit status lookup and makes no claim about automatic callback delivery or acknowledgement. General swap requests do not use this library's `on_callback` mechanism.

Verify the following before adapting these examples:

- Preview all three operations without keys; confirm no remote request occurs and an untrusted account is denied.
- Exercise response handlers with fixtures for success, API rejection, missing fields, inconsistent currencies/amounts and expired white-label details. Check that an invalid response does not become a successful stored result.
- Check that an ordinary repeated confirmation does not submit the same saved attempt again. A response-processing error must not trigger another payout or swap.
- In a separately authorized integration test, compare the provider's track ID, result and final status with the saved attempt. Test unknown outcomes and repeated notifications before adding fulfillment. Local previews and fixture checks do not establish live transport or payment delivery.
