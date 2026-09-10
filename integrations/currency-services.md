---
description: Read and validate cached CurrencyQuote rates, convert an amount, and replace the old free CurrencyConverter endpoint.
---


# Display currency conversions

`CurrencyQuote` exposes cached fiat and cryptocurrency quotes in the runtime. It is separate from the installed `Libs.CurrencyConverter` wrapper and its external free API.

Use quotes for informational conversion. Before displaying a value, check that the currencies exist and that the underlying data is recent enough for your use case.

## Convert an amount

{% code title="Convert an amount · Example 1" overflow="wrap" %}
```javascript
const fiat = CurrencyQuote.fiat;
const crypto = CurrencyQuote.crypto;
if (!fiat || !crypto ||
    typeof fiat.getCachingTime !== "function" ||
    typeof crypto.getCachingTime !== "function") {
  Bot.sendMessage("Quotes are unavailable. Please try later.");
  return;
}
const fiatAge = fiat.getCachingTime();
const cryptoAge = crypto.getCachingTime();
const maxAge = 3600; // Your application's freshness limit, in seconds.
if (!Number.isFinite(fiatAge) || !Number.isFinite(cryptoAge) ||
    fiatAge < 0 || cryptoAge < 0 || fiatAge > maxAge || cryptoAge > maxAge) {
  Bot.sendMessage("Quotes are too old for this estimate.");
  return;
}
if (!fiat.EUR || !crypto.BTC) {
  Bot.sendMessage("A required currency is unavailable.");
  return;
}
const amount = CurrencyQuote.convert({ amount: 15, from: "EUR", to: "BTC" });
Bot.sendMessage("Estimated BTC for 15 EUR: " + amount);
```
{% endcode %}

The freshness limit in this example is your chosen policy, not a promise that every quote refreshes within an hour. Both rates must be valid for a cross-family conversion.

## Reference

- `CurrencyQuote.fiat.SYMBOL` and `CurrencyQuote.crypto.SYMBOL` return available quote values.
- `fiat.getCachingTime()` and `crypto.getCachingTime()` report seconds since their respective updates when initialized.
- `CurrencyQuote.convert({ amount, from, to })` calculates using the available USD-based rates. Missing symbols throw. The current function rejects zero because `amount` is checked for truthiness; handle a zero amount before calling it.
- Provider details may exist under `crypto.details`; do not assume every symbol has a detail record or fixed fields.

The result is not a provider invoice, executable trade, guaranteed exchange rate, or fee calculation. Use the payment provider's actual invoice data when requesting payment.

<details>
<summary>The older CurrencyConverter library</summary>

## The older CurrencyConverter library

The checked Store source calls `http://free.currencyconverterapi.com/api/v5/convert` and maintains its own 30-minute cache. The provider currently reports the free API as down without a promised restoration date. A valid-looking old library therefore does not establish a working data source. [Provider status](https://free.currencyconverterapi.com/).

For existing bots, inventory calls to `Libs.CurrencyConverter.convert`, replace them with a verified quote source and update the callback contract at the same time. The old callback puts the converted value in command parameters, whereas `CurrencyQuote.convert` returns a number immediately. Do not replace one call mechanically while keeping the old callback command.

For another external rate source, use [HTTP](../bjs/http.md), validate its response and timestamp, and define an explicit unavailable-data response.

</details>
