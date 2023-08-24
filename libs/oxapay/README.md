---
cover: ../../.gitbook/assets/botsbusiness.jpg
coverY: 0
---

# OxaPay

With the [OxaPay](https://oxapay.com/?ref=53389) merchant web service, Bots.Business enables you to seamlessly accept crypto payments from your customers and facilitate hassle-free crypto payouts.&#x20;

This integration empowers you to provide efficient, secure, and quick transactions, all without the need for extensive KYC procedures



## Getting Started

To get started with the Bots.business integration with OxaPay, follow these steps:

1\. Generate an OxaPay account and obtain your API key by referring to the [OxaPay Getting Started Guide](https://docs.oxapay.com/getting-started).

2\. Set your generated API keys with the following sample codes:

```javascript
Libs.OxaPayLib.setMerchantKey("YOUR_MERCHANT_KEY");
Libs.OxaPayLib.setPayoutApiKey("YOUR_PAYOUT_API_KEY");
```

For testing purposes, you can use 'sandbox' as the merchant key to access the OxaPay merchant web service in a sandbox environment.

\
Try Out the Sample Bot
----------------------

Experience the convenience of OxaPay integration by trying our sample bot. Install and test the demo bot using the OxapayLibSampleBot library.&#x20;

Visit Store > Crypto > OxapayLibSampleBot to get started.\


## Calling API Methods

You can interact with the OxaPay API by using the \`apiCall\` method. This method accepts three parameters:

\- \`url\`: Specify OxaPay endpoints, such as '/merchants/request' or '/api/send' (refer to the OxaPay [documentation](https://docs.oxapay.com/api-reference) for a full list of endpoints).

\- \`fields\`: Provide an object containing input parameters relevant to the chosen endpoint. Refer to the API documentation for specific details.

\- \`onSuccess\`: Define your custom logic to handle the output of the method.



### Available URLs

Here is a list of commonly used OxaPay endpoints:

\- \`/merchants/request\`: Request crypto payments.

\- \`/merchants/request/whitelabel\`: Request crypto payments with white-label options.

\- \`/merchants/request/staticaddress\`: Request crypto payments with static addresses.

\- \`/merchants/revoke/staticaddress\`: Revoke static addresses.

\- \`/merchants/inquiry\`: Make inquiries about crypto payments.

\- \`/merchants/list\`: Listof your crypto payments.

\- \`/merchants/allowedcoins\`: Get a list of your allowed cryptocurrencies.

\- \`/merchants/rate\`: Check exchange rates.

\- \`/api/send\`: Send crypto payments.

\- \`/api/list\`: List of your pauout transactions.

\- \`/api/balance\`: Check your account balance.

\- \`/api/currencies\`: Get a list of supported cryptocurrencies by OxaPay.

\- \`/api/networks\`: List of supported networks.

\- \`/monitor\`: Monitor OxaPay service availability.



Feel free to explore these endpoints to build powerful crypto payment solutions with Bots.business and OxaPay.

\


### Examples

Explore practical examples of integrating Bots.business with OxaPay.

\


### Creating White-Label Payment

\- Execute the command \`/paytrx\` to create a white-label payment.

\- Provide necessary options such as amount, currency, payCurrency, lifeTime, orderId, and onCallback.

\- The \`onCreatePaymentWithTRX\` command handles the output, generating a QR code and providing payment details.



Command /paytrx

{% code lineNumbers="true" fullWidth="false" %}
```javascript
let options = {
  url: "merchants/request/whitelabel",
  fields: {
    amount: 100,
    currency: "TRX",
    payCurrency: "TRX",
    lifeTime: 90,
    orderId: "ORD-124",
    onCallback: "/onCallbackPayment",
  },
  onSuccess: "/onCreatePaymentWithTRX",
};

Libs.OxaPayLib.apiCall(options);
```
{% endcode %}



command /onCreatePaymentWithTRX

{% code lineNumbers="true" %}
```javascript
if (!options) { return }

if (options.result!= 100) {
  // not success
  Bot.sendMessage(options.message);
  return
}

// result is 100 / success
let toDate = new Date(options.expiredAt * 1000).toISOString();
let caption = 
  "📨Address <code>" + options.address + "</code>" +
  "\<br>—————————" +
  "\<br>Coin" + options.currency +
  "\<br>—————————" +
  "\<br>Network" +
  "\<br>" + options.network +
  "\<br>—————————-" +
  "\<br>Amount" +
  "\<br><code>" + options.payAmount + "</code> " + 
    options.payCurrency + "" +
  "\<br>‼️ Sending less may result fund loss" +
  "\<br>—————————-" +
  "\<br>‼️ Please only send " + options.currency + " on " + options.network +
  "\n network to the address until " + toDate

Api.sendPhoto({
  photo: options.QRCode,
  caption: caption,
  parse_mode: "HTML",
});
```
{% endcode %}



#### Payment Callback

\- When payment status changes, the \`/onCallbackPayment\` command processes the status and notifies users accordingly.



command `/onCallbackPayment`

{% code lineNumbers="true" %}
```javascript
const ADMIN_TELEGRAM_ID = "PUT YOUR TELEGRAM ID HERE";

if (!options) return;

if (options.status == "Confirming"){
  Bot.sendMessage(
    `📢 Your paid ${options.payAmount} ${options.payCurrency} is confirming...`
  );
}else if (options.status == "Paid") {
  Bot.sendMessage(`📢 Your payment was successful`);
}

Api.sendMessage({
  chat_id: ADMIN_TELEGRAM_ID,
  text: "📢 Your invoice with trackId " + 
      `${options.trackId} and orderId ${options.orderId} ${options.status}`
});

```
{% endcode %}



### Creating Payout

\- Use the command `/transfer` to initiate a payout.

\- Specify options like amount, currency, address, and `onCallback`.

\- The `onTransfer` command captures the result, notifying users about the payout status.



commend `/transfer`

{% code lineNumbers="true" %}
```javascript
let amount = 10;
let options = {
  url: "api/send",
  fields: {
    amount: amount,
    currency: "TRX",
    address: "YOUR_PRIVATE_ADDRESS",
    onCallback: "/onCallbackPayout",
  },
  onSuccess: "/onTransfer " + amount +" TRX",
};
Libs.OxaPayLib.apiCall(options);
```
{% endcode %}



command `/onTransfer`

{% code lineNumbers="true" %}
```javascript
if (!options) return;
if (options.result == 100){
  Bot.sendMessage(
    `Send ${params} was submited!\nYour trackId: ${options.trackId}`
  );
} else {
  Bot.sendMessage(`Your send request failed. ${options.message}`);
}

if (options.status == "complete"){
  Bot.sendMessage("Your transfer was successful");
}
```
{% endcode %}



### Payout Callback

The `/onCallbackPayout` command reacts to payout status changes and keeps users informed.



command `/onCallbackPayout`

{% code lineNumbers="true" %}
```javascript
if (!options) return

const ADMIN_TELEGRAM_ID = 'PUT YOUR TELEGRAM ID HERE'

if (options.status == 'Confirming'){
  Bot.sendMessage(`📢 Your withdrawal is confirming...`)
} else if(options.status == 'Complete'){
  Bot.sendMessage(`📢 Your withdrawal was successfully complete!`)
}

Api.sendMessage({
  chat_id: ADMIN_TELEGRAM_ID,
  text: "📢 Your client withdraw " +
   `${options.amount} ${options.currency} ${options.status} `
})
```
{% endcode %}

