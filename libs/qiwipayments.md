---
description: Отслеживайте платежи с помощью Qiwi.com
---

# QiwiПлатежи

## Введение

{% hint style="warning" %}
Необходим Api token с [https://qiwi.com/api](https://qiwi.com/api)
{% endhint %}

### Установите Api Token в библиотеку:

`Libs.QiwiPayment.setQiwiApiTokent(API_KEY);`

### Получите ссылку платежа

```javascript
let link = Libs.QiwiPayment.getPaymentLink({
    account: "+7XXXXXXXXXX", // Qiwi кошелек
    amount: 250, // количество в рублях
    comment: "u" + String(user.id) // отследите транзакцию благодаря метке для пользователя или заказа 
});
```

Пользователь может оплатить через эту ссылку.

### Боту нужно проверить оплату

```javascript
Libs.QiwiPayment.acceptPayment({
      account: "+7XXXXXXXXXX", // Qiwi кошелек
      onSuccess: "/onacceptpayment",
      onNoPaymentYet: "/onnopaymentyet",
      comment: "u" + user.id // отследите транзакцию благодаря метке дя пользователя или заказа
})
```

Если платеж получен то команда `/onacceptpayment` выводитсяя.

Параметры содержат количество в рублях:

`let amount = parseFloat(params);`

