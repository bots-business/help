# CoinPayments \(CP\)

This Lib make integration with [https://www.coinpayments.net](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de) in easy way.

## Initial setup

Need to setup public and private key:

1. [Register](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de)
2. Go to this [page](https://www.coinpayments.net/acct-api-keys) and generate new key.

![](../.gitbook/assets/image%20%2844%29.png)

Press on button "Edit Permissions" and add API Key Permissions:

![Check all options what you need](../.gitbook/assets/image%20%2818%29.png)

Then on bot `/setup` command:

```javascript
// Get your keys in https://www.coinpayments.net/index.php?cmd=acct_api_keys
Libs.CoinPayments.setPrivateKey("YOUR KEY");
Libs.CoinPayments.setPublicKey('YOUR KEY');

// for Receiving Payments
// Get your BB Api Key from Bots.Business App in Profile
Libs.CoinPayments.setBBApiKey('YOUR API KEY');
```

## Call API methods

All CoinPayments API method available [here](https://www.coinpayments.net/apidoc-intro).

For example for method [Get Basic Account Information](https://www.coinpayments.net/apidoc-get-basic-info) we need 2 commands: `/info` and `/onInfo`

`/info` command:

```javascript
Libs.CoinPayments.apiCall({
  fields: { cmd: "get_basic_info"},
  onSuccess: '/onInfo'
});
```

{% hint style="info" %}
In fields you can pass all fields from CoinPayments api. Just read [help](https://www.coinpayments.net/apidoc-intro). 
{% endhint %}

`/onInfo` command:

```javascript
Bot.sendMessage(inspect(options));
Bot.sendMessage("CoinPayments owner email:" + options.body.result.email);
```

## Combine libs!

CoinPayments API do not have some methods. For example get balance by address, validate address, get transactions for address and etc.

Use Block.io [Lib](https://help.bots.business/libs/blockio) with CP Lib together!

Block.io is free if you do not use wallets there.



## Receiving Payments

{% hint style="success" %}
See [demo bot](https://telegram.me/BBDemoStoreBot). Available in the Store.
{% endhint %}

It is possible to receive payment for a temporary or permanent wallet.

**Temporary wallet benefits:**

* Fixed amount
* Can bind the payment to the ordered product
* Status and checkout page
* QR code for payment
* one address for one payment

**Permanent wallet benefits:**

* Any amount
* One address for several payments
* User can transfer funds at any time

### Setup: set IPN Secret

{% hint style="warning" %}
The first step is to go to the [My Settings](https://www.coinpayments.net/index.php?cmd=acct_settings) page &gt; **Merchant Settings**  and set a IPN Secret.

Your IPN Secret is a string of **your choosing**. Recommended to be a random string of letters, numbers, and special characters.

CoinPayments **will not send** any IPNs unless you have an IPN Secret set. 

See [more](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

**Once more!** You need input **any text \(random text\)** as IPN secret in Merchant Settings page

## Temporary wallet

We use command "create\_transaction" with IPN.

Please see [https://www.coinpayments.net/apidoc-create-transaction](https://www.coinpayments.net/apidoc-create-transaction) for details.

Yes, you can write it via `Libs.CoinPayments.apiCall`method too. But there is an easier way.

### 

### Command `/pay`

```javascript
let amount = 0.0001; // amount in BTC

options = {
  fields: {
     amount: amount,   // amount in BTC
     currency: "BTC",  // currency1 = currency2 = BTC
     // currency1: "BTC",   // The original currency of the transaction
     // currency2: "LTC"  //The currency the buyer will be sending
     // you can use another fields also
     // except custom and ipn_url (it used by Lib)
     // See https://www.coinpayments.net/apidoc-create-transaction
  },
  // generated wallet, QR code, payment page
  // will be available in this command
  onSuccess: '/onCreatePayment',
  
  // on successful payment this command
  // will be executed
  onPaymentCompleted: "/onPaymentCompleted",
  
  // it is not necessary
  // onIPN: "/onIPN"
  
  // if you want customize error messages
  // onError: "/onError"
}

Libs.CoinPayments.createTransaction(options);
```

{% hint style="success" %}
#### Automatically with Library:

#### CoinPayments: [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

{% hint style="info" %}
It is preferable to use method  **`onPaymentCompleted`** and not method **`onIPN`**. 

Since the method **`onPaymentCompleted`** completely covers the IPN and solves the problem with [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

### Command `/onCreatePayment`

```javascript
// You can inspect all options:
// Bot.sendMessage(inspect(options));

let result = options.result;

let msg = "*Need pay:*\n `" + result.amount + "`" + 
 "\n\n*to address:*\n" +
 "`" + result.address + "`" +
 "\n\n [Checkout](" + result.checkout_url +
    ") | [Status](" + result.status_url + 
 ")" // you can uncomment this for manual status checking
 // + "\n\nCheck status manually: /check" + options.payment_index;

Bot.sendMessage(msg);
Api.sendPhoto({ photo: result.qrcode_url }); 

```

### Command `/onPaymentCompleted`

This command will be executed on successful payment

{% hint style="info" %}
Need install ResourcesLib
{% endhint %}

```javascript
// you can inspect all options
// Bot.sendMessage(inspect(options));

if(!options){
   // for security we need to check that this command runned only by lib
   // user can not run command with options
   return
}

Bot.sendMessage("Payment completed");

let amount = options.amount1;

let res = Libs.ResourcesLib.userRes("balance");
res.add(amount)

Bot.sendMessage("added to balance, BTC: " + amount);
```

### Finished!

Now you can  receive payments

### Additional Information

You can check payment status

```javascript
Libs.CoinPayments.getTxInfo({
  payment_index: payment_index,  // see /onCreatePayment command.
                                    // Need pass this payment_index to this command
  onSuccess: '/on_txn_id'
})
```

#### `command: /on_txn_id:` 

```javascript
// You can inspect all options:
// Bot.sendMessage(inspect(options));

Bot.sendMessage(options.result.status_text);

// Do not finish payment here.
// Use /onPaymentCompleted for this
```

#### `command /onIPN`

You can get info from IPN. Really it is not needed in simple. Just use onPaymentCompleted option on createTransaction.

```javascript
// You can inspect all fields:
// Bot.sendMessage(inspect(options))

// IPN is not needed
// Use - onPaymentCompleted

Bot.sendMessage("IPN: Payment status: " + options.status_text );

```

#### `command onError`

```javascript
// You can inspect all fields:
Bot.sendMessage(inspect(options))
```

### 

## Permanent wallet

We use command "get\_callback\_address" with IPN.

Please see [https://www.coinpayments.net/apidoc-get-callback-address](https://www.coinpayments.net/apidoc-get-callback-address) for details.

Yes, you can write it via `Libs.CoinPayments.apiCall`method too. But there is an easier way.



### Command `/createWallet`

```javascript
Libs.CoinPayments.createPermanentWallet({
  currency: "BTC",
  //label: "myLabel",
  onSuccess: "/onWalletCreate",
  
  // onIPN - not necessary
  //onIPN: "/onPermanentWalletIPN",
  
  onIncome: "/onIncome"
  
  // if you want customize error messages
  // onError: "/onError"
});
```

{% hint style="success" %}
#### Automatically with Library:

#### CoinPayments: [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

{% hint style="info" %}
It is preferable to use method  **`onIncome`** and not method **`onIPN`**. 

Since the method **`onIncome`** completely covers the IPN and solves the problem with [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)
{% endhint %}

### Command `/onWalletCreate`

```javascript
//Bot.sendMessage(inspect(options));

let wallet = options.result.address;
Bot.sendMessage("Your permanent wallet address is:\n`" + wallet + "`")

// You can save wallet
//User.setProperty("wallet", wallet, "string");
```

### Command `/onIncome`

```javascript
// anybody can run /onIncome command!

if(!options){
   // for security we need to check that this command runned only by lib
   // user can not run command with options
   return
}

let wallet = options.address;
let currency = options.currency;
let amount = options.amount;

let fiat_amount = options.fiat_amount;
let fiat_currency = options.fiat_coin;

let fee = options.fee;

let txn_id = options.txn_id

// see another fields by
// Bot.sendMessage(inspect(options));

Bot.sendMessage(
   "*Income to wallet:*" +
   "\n`"+ wallet + "`" +
   "\n\n*Amount*:\n" +
amount + " " + currency + " (" + fiat_amount + " " + fiat_currency + ")" +
   "\n*Fee*: " + fee +
   "\n\nTXN: `" + txn_id + "`"
);

```



#### `command onError`

```javascript
// You can inspect all fields:
Bot.sendMessage(inspect(options))
```





## Troubleshooting & Debuging

* Do not use same CoinPayment account for receiving and transfering funds.
* Go to [page](https://www.coinpayments.net/index.php?cmd=acct_balances&action=deposits). This list must have history with completed income transaction\(s\)
* Try to resend IPN - see Debuging
* Verify that you have [set IPN secret](https://help.bots.business/libs/coinpayments#set-ipn-secret)

### IPN History

You can view IPN History by link [https://www.coinpayments.net/acct-ipn-history](https://www.coinpayments.net/acct-ipn-history)

![](../.gitbook/assets/image%20%2842%29.png)

Also you can resend IPN by checkin "Resend" checkbox and button "Re-send checked IPN\(s\)"

### Test methods

#### Temporary wallet:

Also it is possible **make test onPaymentCompleted** event. It is good if you do not want make test payment.

```javascript
options = {
  onPaymentCompleted: "/onPaymentCompleted 0.75"
}

Libs.CoinPayments.callTestPaymentCompleted(options);
```

#### 

#### Permanent wallet:

Also it is possible **make test** callTestPermanentWalletIncome event. It is good if you do not want make test payment.

```javascript
options = {
  // onIPN: "/onPermanentWalletIPN",  // if you need IPN also
  onIncome: "/onIncome",
  
  // not necessary options
  // you can pass amount
  //amount: 0.5
  // txn_id: YOUR_TXN_ID
}

Libs.CoinPayments.callTestPermanentWalletIncome(options);
```



## Security

{% hint style="warning" %}
It is strongly recommended to pay attention to safety when using this library.
{% endhint %}

**Do not use default names** for secure commands such as `/onIncome`, `/onPaymentCompleted`

Anybody can run any command by names. So need to check that security command runned by CoinPayment Lib only!

```javascript
if(!options){
   // for security we need to check that this command runned only by lib
   // user can not run command with options
   return
}

// your secure code
...
```

{% hint style="danger" %}
Do not use any non official libs now. 

* Any lib can run command with options.
* Any libs can read properties \(and read your API Keys from other lib\)

We have not way to protect this now. Just **not use NON official libs** with CP lib. Well, that now there are no such libraries
{% endhint %}



**Grant only the really necessary permissions for the Api key.** If yor bot do not need "create\_withdrawal" or etc methods - turn theys off. 

![Check all options what you need](../.gitbook/assets/image%20%2818%29.png)

Read more about security [here](https://help.bots.business/scenarios-and-bjs/bjs-security)

