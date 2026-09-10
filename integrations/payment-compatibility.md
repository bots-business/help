---
description: Review an old payment-library integration before reusing its keys, callbacks, or payout examples.
---

# Review an old payment integration

Older bots may contain `QiwiPayment`, `CoinPayments`, `OxaPayLib`, or provider-specific webhook code. Keep the bot's business records, but verify the service and API generation before reusing the integration in a new bot.

## QiwiPayment

The public legacy `qiwi.js` wrapper generates QIWI payment links and queries payment history with a configured token. This is a legacy integration recipe, not a verified current onboarding path. QIWI Bank's license was revoked on 21 February 2024; the historical wallet instructions must not be presented as if current service availability were established. [Bank of Russia notice](https://www.cbr.ru/eng/press/pr/?id=39708).

If you maintain a bot that uses this wrapper:

1. Find calls to `Libs.QiwiPayment` and the commands that act on successful-payment callbacks.
2. Confirm the actual operator, account availability and supported API with that provider.
3. Stop offering a payment method that your account cannot receive or reconcile.
4. Keep old order/payment references for reconciliation while introducing a supported alternative.
5. Change the payment button, invoice creation, callback verification and fulfillment together; replacing only a library name does not migrate a payment flow.

This guide intentionally does not reproduce the old live-transfer setup as a recommended new integration. It also does not infer that every current service with a similar brand name has the same API or availability.

## Select the correct contract

| Existing code | What to check next |
| --- | --- |
| `Libs.OxaPayLib` | Its older endpoint/field contract; [OxaPayLibV1](oxapay.md) is a separate integration |
| `Libs.OxaPayLibV1` | Lowercase request method, matching key family, signed callback and recorded order |
| `Libs.CoinPayments` | [Legacy versus new CoinPayments platform](coinpayments.md) |
| A raw webhook that changes balances | Provider signature, transaction identity, duplicate handling and expected amount |

## Test the complete result

Use provider sandbox facilities where available. Check a successful payment, a pending payment, a canceled/expired invoice, an invalid signature, a repeated callback and a mismatch between requested and received values. Keep invoice creation separate from fulfillment: creating a link has not transferred money.

Never put keys or a transfer endpoint in a public repository, and do not copy old unrestricted payout commands into a public bot. For implementation details, use [Webhooks](../libraries/webhooks.md), [HTTP](../bjs/http.md) and the selected provider's current documentation.
