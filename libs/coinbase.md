# Coinbase (CB)



This Lib make integration with [coinbase.com](http://coinbase.com) in easy way.

## Initial setup

1. [Register](https://www.coinbase.com/signup)
2. Go to this [page](https://www.coinbase.com/settings/api) and generate new key. Select needed rights:

![](<../.gitbook/assets/image (69).png>)

Install lib and make `/setup` command with code:

```javascript
Libs.Coinbase.setup();
```

3\. Go to Admin Panel and fill Api Key and Secret Api Key:

![](<../.gitbook/assets/image (71).png>)

4\. If you need notifications copy Notifications url and fill it on step 2

5\. Fill command to be called on notifications if you need it

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
  // apiKey - if you need custom Api Key
  // secretApiKey - if you need custom Api Key
})
```

{% hint style="info" %}
In body you can pass all fields from Coinbase api.
{% endhint %}

`/onApi` command:

```javascript
Bot.inspect(options.result)
```



`/onApiError` command:

```javascript
Bot.sendMessage(
  "We have error with Coinbase API. Please try later. " +
  options.error // error message from Coinbase Api
)

Bot.sendMessage(
  options.http_status + " " + JSON.stringify(options.result)
)
```

{% hint style="warning" %}
if you do not have onError command error will be tracked in Eror Tab.&#x20;
{% endhint %}



## Typical errors

![](<../.gitbook/assets/image (96) (1).png>)

You need to setup Api key. Please see step 3 in [setup](coinbase.md#initial-setup).
