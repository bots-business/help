---
description: Identify the CoinPayments API generation used by the existing library and test a read-only legacy request.
---

# Use an existing CoinPayments integration

The checked `Libs.CoinPayments` library targets the **legacy** `https://www.coinpayments.net/api.php` form API. CoinPayments also has a newer JSON API with different hosts, credentials and contracts. Choose the documentation for your account's platform before configuring this library. [CoinPayments platform guide](https://docs.coinpayments.net/api/).

## Check credentials without creating a payment

Install `CoinPayments` for a test bot. From an owner-only setup command, configure its legacy public and private keys:

```javascript
Libs.CoinPayments.setPublicKey("YOUR_LEGACY_PUBLIC_KEY");
Libs.CoinPayments.setPrivateKey("YOUR_LEGACY_PRIVATE_KEY");
```

Remove literal keys from the command after setup and keep these values out of exported public code. In an owner-only `/coinpayments-check` command:

```javascript
Libs.CoinPayments.apiCall({
  fields: { cmd: "get_basic_info" },
  onSuccess: "/coinpayments-check-result"
});
```

In `/coinpayments-check-result`:

```javascript
if (!options || !options.body) { return; }
if (options.body.error !== "ok") {
  Bot.sendMessage("CoinPayments rejected the account check.");
  return;
}
Bot.sendMessage("Legacy CoinPayments credentials are accepted.");
```

This method reads account information; it does not create a transaction. The legacy API identifies the operation with `cmd` and returns `error: "ok"` on success. [Legacy account check](https://www.coinpayments.net/apidoc-get-basic-info).

The wrapper uses **`onSuccess`**, unlike OxaPay's `on_success`. The parsed reply is under **`options.body`**. It signs the outgoing form body with HMAC-SHA512. Do not reuse the newer platform's authentication recipe with this wrapper.

## Existing payment flows

The library also exposes `createTransaction`, `getTxInfo`, `createPermanentWallet`, and callback helpers. Its transaction convenience flow constructs a webhook containing a BB API key, and the checked IPN handler is not a complete provider-signature verification boundary. Do not connect a callback directly to valuable balance crediting merely because it reached a bot command.

For an existing deployment, inventory its installed source, API generation, callback URLs and payout permissions. Validate callbacks against the provider's matching authentication rules and recorded transaction state before fulfillment. The newer platform requires a fresh integration; changing the URL in the legacy wrapper is insufficient.

## Errors and migration

The wrapper records API errors and rejects further calls for five minutes after an error. Fix the underlying key, endpoint or request issue instead of repeatedly retrying. A missing result, network failure or an `error` value other than `ok` must not be treated as success.

For new integrations, begin with the provider's current account-specific API documentation and [BJS HTTP](../bjs/http.md), or use the separately documented [OxaPay test flow](oxapay.md). The presence of legacy source is not a promise that your particular legacy account is still supported.
