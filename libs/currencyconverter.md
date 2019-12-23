---
الوصف: التحويل سهل مثل بضعة سطور من الكود!
---

# محول العملات

يتم تحديث قيم العملة كل 60 دقيقة.

## مجانا

* بحد أقصى 100 طلب / ساعة
* مشترك مع جميع المستخدمين الذين يتفوقون عليه مجانًا.
* تعطل عندما تكون هناك حاجة إلى إعادة تشغيل الخادم لإصلاح الأخطاء والتحسينات

## مثال بوت
@DemoCurrencyConverterBot

## رمز المثال

في أي أمر:

```javascript
let amount = 1;
let onSucces = '/onconvert';
let conversation = 'USD_EUR' // others: USD_BTC, BTC_USD, CNY_BTC and etc...
Libs.CurrencyConverter.convert(conversation, amount, onSucces);
```

### في القيادة
###'/onconvert':

```javascript
// النتيجة المخزنة في params
Bot.sendMessage(params);
```



