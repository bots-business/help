# CoinPayments \(CP\)

## Lib in progress...

This Lib make integration with [https://www.coinpayments.net](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de) in easy way.

## Initial setup

Need to setup public and private key:

1. [Register](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de)
2. Go to this [page](https://www.coinpayments.net/acct-api-keys) and generate new key.

Then on /setup command:

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

## Receiving Payments

We use command "create\_transaction" with IPN

Please see [https://www.coinpayments.net/apidoc-create-transaction](https://www.coinpayments.net/apidoc-create-transaction) for details.

### Command `/order`

```javascript
let orders = User.getProperty("orders");
if(!orders){ orders = { count: 0, list:{} } };
orders.count = orders.count + 1;
orders.list[orders.count] = { status: "new" }
User.setProperty("orders", orders, "json")

Libs.CoinPayments.apiCall({
  fields: {
     cmd: "create_transaction",
     // see fields description in
     // https://www.coinpayments.net/apidoc-create-transaction
     amount: 0.001,
     currency1: "BTC",
     currency2: "BTC",
     buyer_name: user.telegramid,
     item_name: "ItemName001",
     ipn_url: Libs.CoinPayments.getIpnUrl("/onIPN"),
     
     // it is not necessary
     item_number: "001",
     invoice: orders.count,
     custom: "custom - field",
     
  },
  onSuccess: '/onCreateOrder'
});
```

{% hint style="danger" %}
#### CoinPayments: [IPN Retries / Duplicate IPNs](https://www.coinpayments.net/merchant-tools-ipn)

If there is an error sending your server an IPN, we will retry up to 10 times.

Because of this you are not guaranteed to receive every IPN \(if all 10 tries fail\) or that your server will receive them in order.

  
Your IPN handler must always check to see if a payment has already been handled before to avoid double-crediting users, etc. in the case of duplicate IPNs.

So we **must pass unique** item\_name, item\_number, invoice, or custom field. 
{% endhint %}

### Command `/onCreateOrder`

```javascript
// You can inspect all options:
// Bot.sendMessage(inspect(options));

let err = options.body.error;
if(err!="ok"){
  Bot.sendMessage("CoinPayments error:\n\n" + err);
  return
}

let result = options.body.result;

let msg = "*Need pay:*\n `" + result.amount + "`" + 
 "\n\n*to address:*\n" +
 "`" + result.address + "`" +
 "\n\n [Checkout](" + result.checkout_url + ") | [Status](" + result.status_url + ")";

Bot.sendMessage(msg);
Api.sendPhoto({ photo: result.qrcode_url });
```

### Command `/onIPN`

Doc in progress



