---
الوصف: تتبع الدفع مع Qiwi.com
---

# Qiwi Payments
## الشروع في العمل

{% hint style="warning" %}
تحتاج Api token من 
[https://qiwi.com/api](https://qiwi.com/api)
{% endhint %}

### Set Api Token to lib:

`Libs.QiwiPayment.setQiwiApiTokent(API_KEY);`

### احضار رابط الدفع

```javascript
let link = Libs.QiwiPayment.getPaymentLink({
    account: "+7XXXXXXXXXX", // Qiwi wallet
    amount: 250, // amount in RUB
    comment: "u" + String(user.id) // تتبع المعاملات مع التسمية للمستخدم أو النظام 
});
```

يمكن للمستخدم إجراء الدفع عبر هذا الرابط.

 ### بوت بحاجة إلى مدفوعات الشيكات

```javascript
Libs.QiwiPayment.acceptPayment({
      account: "+7XXXXXXXXXX", // Qiwi wallet
      onSuccess: "/onacceptpayment",
      onNoPaymentYet: "/onnopaymentyet",
      comment: "u" + user.id // تتبع المعاملات مع التسمية للمستخدم أو النظام
})
```

إذا تلقى الدفع القيادة
`/onacceptpayment` نفذ.

تحتوي البارامس على مبلغ في RUB:
`let amount = parseFloat(params);`

