---
cover: ../.gitbook/assets/botsbusiness.jpg
coverY: 0
---

# OxaPay

With the [OxaPay](https://oxapay.com/?ref=53389) merchant webservice, you can accept crypto payments from your customers in a fast, secure and hassle-free way, without the need for KYC.&#x20;

Try out the sample bot and see for yourself how OxaPay can simplify your crypto payment experience



{% hint style="success" %}
You can install and test **demo bot** with this lib.&#x20;

Please see Store > Crypto > OxapayLibSampleBot
{% endhint %}



## Initial setup <a href="#_rcp7sut13qql" id="_rcp7sut13qql"></a>

1. [Register](https://oxapay.com/?ref=53389) in OxaPay
2. Log in to your OxaPay account and create a merchant key by filling in the fields and selecting accepted coins on the merchants [page](https://account.oxapay.com/merchants).
3. In “your merchant keys” table, copy the generated merchant key.
4. Install OxaPay lib and write /setup command that run the below code:

`Libs.OxaPayLib.setMerchantKey("YOUR_MERCHANT_KEY");`

{% hint style="success" %}
You can use ‘oxapay’ as the merchant key to test the merchants webservice (Crypto Payment Gateway).
{% endhint %}

Just run the bot and execute this command `/setup`. Then you can remove it.





## Methods

| setMerchantKey                      | Set your merchant key to setup the merchant gateway                                                                 |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| <p>createTransaction</p><p><br></p> | Register order informations and returns payment info such as trackId and payLink to connect OxaPay payment gateway. |
| getTxInfo                           | Returns report of a payment session such as status, pay time, amount, etc.                                          |
| getAcceptedCoins                    | Returns the list of your merchant's accepted coins.                                                                 |

### &#x20;<a href="#_rcp7sut13qql" id="_rcp7sut13qql"></a>

\


## Receiving Payments <a href="#_l95wxke42czb" id="_l95wxke42czb"></a>

To create a payment link, you must first install [Webhook Lib](webhooks-lib.md) from the Libs and then receive your payment link by executing the createTransaction method with sending payment parameters!

The payment parameters are as follows:

| Amount (require) | Float  | Total amount (as dollar if currency filed was not filled. Otherwise amount should be filled with your specified currency amount)                                        |
| ---------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| currency         | String | If you want that your invoice to be paid by a specified currency fill in this field with its symbols (Find the list of currencies symbol in the accepted coins section) |
| description      | String | Order details (will be shown in different reports)                                                                                                                      |
| orderId          | String | Your unique order ID (optional - used in reports)                                                                                                                       |
| email            | String | User's email for the report                                                                                                                                             |

\


The above parameters are set in the \`fields\` property and `onCreatePayment` property is also needed to be run after creating the payment. It will return the payment information including the following fields.

\


| result  | Request result. [Read more](https://docs.oxapay.com/merchants-payment-gateway#result-table-1)                                                                                  |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| message | The message containing the result of the request.                                                                                                                              |
| trackId | The unique identifier of each payment session of the OxaPay payment gateway, which can be used to query the payment status and report requests (if the request be successful). |
| payLink | The set payment page link according to the trackId field (if the request is successful).                                                                                       |

\


You need to define the command that should be called when the payment is created and in that command, you will get the above information.

You also need the onCompletePayment command to get the transaction information after the payment has been made to the address.



#### For example: <a href="#_qaw5xjh1ngjg" id="_qaw5xjh1ngjg"></a>

**Command /pay**

<pre class="language-javascript"><code class="lang-javascript">let amount = 100; // amount in TRX

options = {
    fields: {
<strong>        amount: amount, // amount in TRX
</strong>        // you can set the rest of the fields as optiona
        // currency: "trx",
        // description: "Hello World!",
        // orderId: "AB-1122",
        // email: "example@gmail.com"
    },
    onCreatePayment: '/onCreatePayment',
}

Libs.OxaPayLib.createTransaction(options);
</code></pre>



**Command /onCreatePayment**

```javascript
// for security we need to check that this command runned only by lib
// user can not run command with options
if (!options) return

// You can inspect all options:
Bot.inspect(options)

// You can send message with pay link
Bot.sendMessage("Payment link:\n" + options.payLink)

// You can send message with web app
Api.sendMessage({
  text: "Pay in web app",
  reply_markup: {
    inline_keyboard: [[{ text: "Pay", web_app: { url: options.payLink } }]]
  }
})

// You can store this information for future inquiries
```



**Command /onCompletePayment**

```javascript
// for security we need to check that this command runned only by lib
// user can not run command with options
if (!options) return

// You can inspect all options or send related message to user
Bot.inspect(options)

Bot.sendMessage("Payment completed")
```



## Transaction Info <a href="#_hctqkgy4fl35" id="_hctqkgy4fl35"></a>

You can query the entire payment information by the getTxInfo method. for this purpose, you need payment trackId. The information includes the following fields.\


| message     | The message containing the result of the request                                                                                                                       |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| result      | Request result. [Read more](https://docs.oxapay.com/merchants-payment-gateway#result-table-2)                                                                          |
| paidAt      | Order payment date - in ISODate format (if the payment is successful)                                                                                                  |
| verifiedAt  | Order verify date - in ISODate format (if the payment is successful)                                                                                                   |
| status      | Payment status (you can check the status values through the status table)                                                                                              |
| amount      | Order amount (as dollar, if currency filed was not filled. Otherwise, amount should be filled with your specified currency amount)                                     |
| currency    | If you want that your invoice to be paid by a specified currency fill in this field with its symbol (Find the list of currencies symbol in the accepted coins section) |
| orderId     | Order ID (if the payment is successful and sent in the request gateway)                                                                                                |
| description | Transaction description (if the payment is successful and sent in the request gateway)                                                                                 |
| email       | User's email for report (if the payment is successful and sent in the request gateway)                                                                                 |
| wage        | How to deduct fees 0: deduction from the transaction, 1: Deduction from the fee wallet, 2: In charge of the customer                                                   |
| createdAt   | Order create date - in ISODate format                                                                                                                                  |
| transaction | <p>This property contains 3 fields:<br>Coin // Paid currency symbol</p><p>Amount // Paid currency amount</p><p>txId // Paid transaction hash</p>                       |



#### Transactions status table

| -3 | Canceled by the user |
| -- | -------------------- |
| -2 | Unsuccessful payment |
| -1 | Payment pending      |
| 1  | Paid, verified       |
| 2  | Paid, Not verified   |



#### For example: <a href="#_m1sqzhtrg668" id="_m1sqzhtrg668"></a>

**Command /txInfo**

```javascript
options = {
  fields: {
    trackId: YOUR_TRACK_ID
  },
  onSuccess: "/onTxInfo"
}

Libs.OxaPayLib.getTxInfo(options)
```

\
**Command /onTxInfo**

```javascript
// for security we need to check that this command runned only by lib
// user can not run command with options
if(!options) return


// You can inspect all options
Bot.inspect(options)
```

\


## Accepted coins <a href="#_jqlj0m8g0vjx" id="_jqlj0m8g0vjx"></a>

Use `getAcceptedCoins` method to get the list of your merchant's accepted coins.



#### For example: <a href="#_2qbe8tcpwul1" id="_2qbe8tcpwul1"></a>

**Command /acceptedCoins**

```javascript
Libs.OxaPayLib.getAcceptedCoins({ onSuccess: "/onAcceptedCoins" })
```

\


**Command /onAcceptedCoins**

```javascript
// for security we need to check that this command runned only by lib
// user can not run command with options
if(!options) return

Bot.sendMessage("Your merchant's accepted coins are " + options.allowed.join(','))

// You can also send the list to the user to choose the coins for payment
```



\
