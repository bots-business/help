---
description: Conversion is as easy as a few lines of code!
---

# CurrencyConverter

Currency values are refreshed every 60 minutes.

## FREE

* Maximum of 100 requests/hr
* Shared with all users who are acccessing it for free.
* Downtime when there's a need to restart the server for bug fixes and enhancements

## Example bot @DemoCurrencyConverterBot

## Example code

In any command:

```javascript
let amount = 1;
let onSucces = '/onconvert';
let conversation = 'USD_EUR' // others: USD_BTC, BTC_USD, CNY_BTC and etc...
Libs.CurrencyConverter.convert(conversation, amount, onSucces);
```

### In command '/onconvert':

```javascript
// result stored in params
Bot.sendMessage(params);
```



