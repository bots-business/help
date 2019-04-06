# CoinPayments \(CP\) - in progress..

## Lib in progress...

This Lib make integration with [https://www.coinpayments.net](https://www.coinpayments.net) in easy way.

## Initial setup

Need to setup public and private key. Go to this [page](https://www.coinpayments.net/acct-api-keys) and generate new key.

Then on /setup command:

```javascript
Libs.CoinPayments.setPrivateKey("PRIVATE KEY");
Libs.CoinPayments.setPublicKey('PUBLIC KEY');
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



