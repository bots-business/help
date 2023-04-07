# OxaPay: Transfer

### Transfer Funds

To transfer funds from your account, you must first create a payment API key from the Payment API page in the OxaPay panel ([here](https://account.oxapay.com/paymentapi)) and then set it in the bot with this code:\


```javascript
Libs.OxaPayLib.setPaymentApiKey("YOUR_PAYMENT_API_KEY");
```



You can send the assets of your OxaPay wallet to another account. This feature is only available for OxaPay users who register with OxaPay and have a unique address as OxaPay address (starting with OXA). You can send any funds (Bitcoin, Ethereum, Tron, etc.) through this address without any fee.

Read more about internal transfer [here](https://docs.oxapay.com/internal-payment-api)

\


After setting the payment API key, you can submit a request by executing the transfer method with these parameters:\
\


| amount      | Float  | Currency amount                                                |
| ----------- | ------ | -------------------------------------------------------------- |
| currency    | String | Currency for transfer                                          |
| address     | String | OxaPay wallet address of receiver (Always must start with OXA) |
| description | String | description of transaction (maximum 200 characters)            |

\


The above parameters are set in the `` `fields` `` property and `onTransfer` property must be run after the transfer request. It will return a response including the following fields.

\


\


| result  | Request result.[ Read more](https://docs.oxapay.com/internal-payment-api#result-table) |
| ------- | -------------------------------------------------------------------------------------- |
| message | The message contains the result of the request.                                        |
| trackId | The unique identifier of each internal transfer.                                       |

### For example

Command `/withdraw`

```javascript
options = {
    fields: {
        amount: 100, // amount in TRX
        currency: "trx",
        address: “OXA…”
        // you can set description field as optional
        // description: "Hello World!",
    },
    onTransfer: “/onTransfer 100 TRX”,
}
Libs.OxaPayLib.transfer(options);
```



Command `/onTransfer`

```javascript
if (!options) return

if (options.result == 100)
    Bot.sendMessage(params + " was withdrawn successfully!")
else if (options.result == 104)
    Bot.sendMessage(options.message)
else {
    Bot.sendMessage("Your withdrawal request failed. Please try again later")
}
```

\


\
