# Coinbase \(CB\)



This Lib make integration with [coinbase.com](http://coinbase.com) in easy way.

## Initial setup

1. [Register](https://www.coinpayments.net/index.php?ref=5418303a5fc165090ee8a9177a3982de)
2. Go to this [page](https://www.coinbase.com/settings/api) and generate new key. Select needed rights:

![](../.gitbook/assets/image%20%2867%29.png)

Install lib and make `/setup` command with code:

```javascript
Libs.Coinbase.setup();
```

3. Go to Admin Panel and fill Api Key and Secret Api Key:

![](../.gitbook/assets/image%20%2866%29.png)

4. If you need notifications copy Notifications url and fill it on step 2

## Call API methods

All Coinbase API method available [here](https://developers.coinbase.com/api/v2#users).

For example for method [create new address](https://developers.coinbase.com/api/v2#create-address) we need 2 commands: `/create` and `/onCreate`

`/create` command:

```javascript
let account_id = "YOU ACCOUNT ID"
// you can see your all accounts in https://www.coinbase.com/accounts/
// just select needed account and copy it ID from url:
// https://www.coinbase.com/accounts/ID

Libs.Coinbase.apiCall({
  method: "POST",   // method can be GET and POST
  path: "accounts/" + account_id + "/addresses",
  body: { name: "myAddress" },
  onSuccess: "/onApi",
  // onError: "/onApiError"  // onError command
  // background: true // perform api call in background for big timeout limit
})
```

{% hint style="info" %}
In fields you can pass all fields from Coinbase api.
{% endhint %}

`/onCreate` command:

```javascript
Bot.inspect(options.content)
```

## 



