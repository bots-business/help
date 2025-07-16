---
cover: ../../.gitbook/assets/botsbusiness.jpg
coverY: 0
---

# OxaPay

## 🔐 Introduction

**[OxaPay](https://oxapay.com/?ref=53389)** is a fast, secure, and developer-friendly **crypto payment gateway** that allows businesses and platforms to seamlessly accept, send, and swap cryptocurrencies.
This integration empowers you to provide efficient, secure, and quick transactions, all without the need for extensive KYC procedures.

### With OxaPay, you can:

- ✅ Accept crypto payments from customers  
- 💵 Send payouts to users, freelancers, or partners  
- 🔁 Instantly swap between cryptocurrencies  
- 🔧 Integrate everything easily via RESTful APIs
---

## 🌐 Key Features

### 💸 Payment
**Accept Crypto Payments from Customers**  
Seamlessly accept cryptocurrency payments via auto generated wallet addresses or invoice links.

- Instant wallet address generation  
- White-label support for custom branding
- Invoice link
- Real-time webhook notifications  
- Multi-currency & multi-network support

---

### 💵 Payout  
**Send Crypto Instantly**  
Automate payouts in crypto to anyone, anywhere.

- Fast and secure transfers  
- Real-time webhook notifications  
- Great for affiliate systems, rewards, freelancers

---

### 🔁 Swap  
**Instant Crypto Exchange**  
Convert one crypto to another instantly and securely.

- Real-time rates  
- Transparent conversion  
- Supports multiple crypto

---

## ⚡️ Why OxaPay?

- ✅ No complex KYC  
- 🔐 Secure & reliable API  
- 🚀 Fast setup and integration  
- 📞 24/7 dedicated support
---

> 🔗 Learn more at [oxapay docs](https://docs.oxapay.com/)

## 🚀 Getting Started

To get started with the Bots.business integration with OxaPay, follow these steps:

1. Generate an OxaPay account and obtain your API key by referring to the [OxaPay Integrations](https://docs.oxapay.com/introduction/integrations).

2. Set your generated API keys with the following sample codes:

```javascript
//set your merchant api key
Libs.OxaPayLibV1.setMerchantApiKey("YOUR_MERCHANT_KEY");

//set your payout api key
Libs.OxaPayLibV1.setPayoutApiKey("YOUR_PAYOUT_API_KEY");

//set your general api key
Libs.OxaPayLibV1.setGeneralApiKey("YOUR_GENERAL_API_KEY");
```
3. Install the ***Webhook Library*** from the Bots.business library store to receive real-time payment and payout notifications (callback data).

## ⚙️ Calling API Method

You can interact with the OxaPay API by using the `apiCall` method. This method accepts four parameters:

- `url`: Specify OxaPay endpoints, such as '/payment/invoice' or '/payout' (refer to the [OxaPay documentation](https://docs.oxapay.com/api-reference) for a full list of endpoints).

-   `method`: Specify the HTTP method for the request, either `POST` or `GET`, depending on the API endpoint requirements.
 
- `fields`: Provide an object containing input parameters relevant to the chosen endpoint. Refer to the API documentation for specific details.

- `on_success`: Define your custom logic to handle the output of the method.

## 🌐 Available URLs
### 💸 Payment Endpoints

| Endpoint                     | Method | Description                   |
|------------------------------|--------|------------------------------|
| /payment/invoice              | POST   | Create a payment invoice |
| /payment/white-label          | POST   | Generate white-label payment  |
| /payment/static-address       | POST   | Create static wallet address  |
| /payment/static-address/revoke| POST   | Revoke a static address       |
| /payment/{track_id}           | GET    | Retrieve payment details by track ID  |
| /payment                     | GET    |  Get a list of all your payment records              |
| /payment/accepted-currencies | GET    | List of your allowed cryptocurrencies |
 
### 💵 Payout Endpoints

 | Endpoint              | Method | Description                              |
|-----------------------|--------|------------------------------------------|
| /payout             | POST   | Send crypto payments              |
| /payout/{track_id}  | GET    | Retrieve payout details by track ID      |
| /payout             | GET    | Get a list of all your payout records    |

### 🔁 Swap Endpoints

| Endpoint                    | Method | Description                                         |
|-----------------------------|--------|-----------------------------------------------------|
| /general/swap            | POST   | Create a new crypto swap request                   |
| /general/swap            | GET    | Get a list of your swap history                    |
| /general/swap/pairs      | GET    | Get a list of supported currency swap pairs        |
| /general/swap/calculate  | POST   | calculate output amount for a given swap request    |
| /general/swap/rate       | POST   | Get real-time exchange rate between two currencies |

### 📚 Common Endpoints

| Endpoint                        | Method | Description                                      |
|---------------------------------|--------|--------------------------------------------------|
| /general/account/balance      | POST   | Get your current account balance                 |
| /common/prices                | GET    | Get real-time crypto prices                      |
| /common/currencies           | GET    | List supported cryptocurrencies                  |
| /common/fiats                 | GET    | List supported fiat currencies                   |
| /common/networks              | GET    | List supported blockchain networks               |




Feel free to explore these endpoints to build powerful crypto payment solutions with Bots.business and OxaPay.

## 📝 Examples

Explore practical examples of integrating Bots.business with OxaPay.

### Payment Example: Create White Label

- Execute the `paytrx` command to create a white-label payment.

- Provide necessary options such as amount, currency, pay_currency, lifetime, order_id, and on_callback.

- The `onCreatePaymentWithTRX` command handles the output, generating a QR code and providing payment details.

Command ***/payTrx***

```javascript
let options = {
	url: "/payment/white-label",
	method: "POST",
	fields: {
		amount: 100,
		currency: 'USD',
		pay_currency: "TRX",
		network: "TRC20",
		lifetime: 60,
		fee_paid_by_payer: 1,
		under_paid_coverage: 20,
		to_currency: "USDT",
		auto_withdrawal: false,
		email: "customer@oxapay.com",
		order_id: "ORD-12345",
		description: "Order #12345",
		on_callback: "/onCallbackPayment"
	},
	on_success: "/onCreatePaymentWithTRX"
}
Libs.OxaPayLibV1.apiCall(options)
```
Command ***/onCreatePaymentWithTRX***

```javascript
if (!options) { return }

if (options.status!= 200) {
  // not success
  Bot.sendMessage(options.error?.message || options.message);
  return
}

let toDate = new Date(options.data.expired_at * 1000).toISOString();

let caption = 
  "📨 Address: <code>" + options.data.address + "</code>" +
  "<br>💰 Coin: <b>" + options.data.currency + "</b>" +
  "<br>🌐 Network: <b>" + options.data.network + "</b>" +
  "<br>💵 Amount: <code>" + options.data.pay_amount + "</code> " + options.data.pay_currency +
  "<br><br>‼️ Sending less may result in fund loss!" +
  "<br>‼️ Please only send <b>" + options.data.currency + "</b> on <b>" + options.data.network + "</b> network." +
  "<br>⏰ Expiry: " + toDate;

Api.sendPhoto({
  photo: options.data.qr_code,
  caption: caption,
  parse_mode: "HTML",
});
```
#### Payment Callback

- When payment status changes, the `/onCallbackPayment` command processes the status and notifies users accordingly.

Command ***/onCallbackPayment***

```javascript
if (!options) return;

const ADMIN_TELEGRAM_ID = "PUT YOUR TELEGRAM ID HERE";

if (options.status == "paying"){
  Bot.sendMessage(
    `📢 Your paid ${options.amount} ${options.currency} is confirming...`
  );
}else if (options.status == "paid") {
  Bot.sendMessage(`✅ Your payment was successful.`);
}

Api.sendMessage({
  chat_id: ADMIN_TELEGRAM_ID,
  text: "📢 Your invoice with trackId " + 
      `${options.track_id} and orderId ${options.order_id} ${options.status}`
});
```

### Payout Example: Generate Payout

- Use the `transfer` command  to initiate a payout.

- Specify options like amount, currency, address, and on_callback.

- The `onTransfer` command captures the result, notifying users about the payout status.

Commend ***/transfer***

```javascript
let amount = 10;
let options = {
  url: "/payout",
  method: "POST",
  fields: {
    amount: amount,
    currency: "TRX",
    network: "TRC20",
    address: "RECEIVER_ADDRESS",
    on_callback: "/onCallbackPayout",
    description: "Order #12345"
  },
  on_success: "/onTransfer " + amount +" TRX",
};
Libs.OxaPayLibV1.apiCall(options);
```
Command ***/onTransfer***

```javascript
if (!options) return;
if (options.status == 200){
  Bot.sendMessage(
    `✅ Send request submitted successfully!\nTrack ID: ${options.data.track_id}`
  );
} else {
  Bot.sendMessage(`❌ Your send request failed. ${options.error?.message || options.message}`);
}

if (options.data.status == "confirmed"){
  Bot.sendMessage("✅ Your transfer was successful.");
}
```

### Payout Callback

- The `/onCallbackPayout` command reacts to payout status changes and keeps users informed.

Command ***/onCallbackPayout***

```javascript
if (!options) return

const ADMIN_TELEGRAM_ID = 'PUT YOUR TELEGRAM ID HERE'

if (options.status == 'Confirming'){
  Bot.sendMessage(`📢 Your withdrawal is confirming...`)
} else if(options.status == 'Confirmed'){
  Bot.sendMessage(`✅ Your withdrawal was successfully completed!`)
}

Api.sendMessage({
  chat_id: ADMIN_TELEGRAM_ID,
  text: `📤 Withdrawal Alert:\nAmount: ${options.amount} ${options.currency}\nStatus: ${options.status}`
})
```
### Swap Example: Swap Request

- Execute the command `swapBtc`.

- Provide necessary options such as amount, from_currency and to_currency.

- The `onSwapResponse` command will handle the API response.

Command ***/swapBtc***
```javascript
let options = {
  url: "/general/swap",
  method:"POST",
  fields: {
	amount: 0.5,
	from_currency: "BTC",
	to_currency: "USDT",
  },
  on_success: "/onSwapResponse",
};
Libs.OxaPayLibV1.apiCall(options);
```
Command ***/onSwapResponse***
```javascript
if (!options) return;
if (options.status == 200){
  Bot.sendMessage(
    `✅ Your swap request was successful!\nTrack ID: ${options.data.track_id}`
  );
} else {
  Bot.sendMessage(`❌ Your swap request failed. ${options.error?.message || options.message}`);
}
```
