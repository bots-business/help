# CurrencyQuote

JavaScript library of currency quotes. It is **included by default** - installation is not required.

It is have currencies and crypto currencies \(top 100 list\)

**Update time:**

* currencies update time is: once at 1 hour
* crypto currencies update time is: once at 5 minutes

{% hint style="warning" %}
Possibly longer update times due to network errors and other errors.

But you can [check](currencyquote.md#check-last-updated-time) this.
{% endhint %}

## Usage

Get prices:

```javascript
// get EUR / USD cost
var eur_price = CurrencyQuote.fiat.EUR;
var btc_price = CurrencyQuote.crypto.BTC;
```

Get crypto details:

```javascript
// get EUR / USD cost
var btcDetails = CurrencyQuote.crypto.details.BTC;
Bot.inspect(btcDetails)
```

Convertation:

```javascript
// convert 15 EUR to TRX
var trx = CurrencyQuote.convert({ amount: 15, from: "EUR", to: "TRX" })

// from 0.1 BTC to INR
var inr = CurrencyQuote.convert({ amount: 0.1, from: "BTC", to: "INR" })
```

## Check last updated time

```javascript
// update time in seconds
// max time in normal 3600 seconds
var secsFiat = CurrencyQuote.fiat.getCachingTime();
var secsCrypto = CurrencyQuote.crypto.getCachingTime();

var oneHour = 60*60;
if(secsCrypto > oneHour){
   // it is very old data! Last updated a hour ago
   Bot.sendMessage("Please try later")
   return
}

var oneDay = oneHour*24;
if(secsFiat > oneDay){
   // it is very old data! Last updated a day ago
   Bot.sendMessage("Please try later")
   return
}

// get EUR
var eur_price = CurrencyQuote.fiat.EUR;
var btc_price = CurrencyQuote.crypto.BTC;
```



