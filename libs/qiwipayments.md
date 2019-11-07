---
description: Track payment with Qiwi.com
---

# QiwiPayments

## Getting started

{% hint style="warning" %}
Need Api token from [https://qiwi.com/api](https://qiwi.com/api)
{% endhint %}

### Set Api Token to lib:

`Libs.QiwiPayment.setQiwiApiTokent(API_KEY);`

### Get payment link

```javascript
let link = Libs.QiwiPayment.getPaymentLink({    account: "+7XXXXXXXXXX", // Qiwi wallet    amount: 250, // amount in RUB    comment: "u" + String(user.id) // track transaction with label for user or order });
```

User can make payment via this link.

### Bot need check payments

```javascript
Libs.QiwiPayment.acceptPayment({      account: "+7XXXXXXXXXX", // Qiwi wallet      onSuccess: "/onacceptpayment",      onNoPaymentYet: "/onnopaymentyet",      comment: "u" + user.id // track transaction with label for user or order})
```

If payment recived command `/onacceptpayment` executed.

Params contain amount in RUB:

`let amount = parseFloat(params);`

