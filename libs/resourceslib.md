---
الوصف: مع lib يمكننا إدارة أي موارد في الروبوت.
---

# ResourcesLib

## الموارد يمكن أن يكون

* توازن \(في USD ، BTC أو أي دولة أخرى\)
* أي موارد اللعبة: الذهب ، الغابة ، الحجر ، الخ
 * الخ ، أي قيم تعويم

## مورد المستخدم

```javascript
let res = Libs.ResourcesLib.userRes("money");
Bot.sendMessage("Cur your money: " + res.value());
```

{% hint style="danger" %}
اسم الدقة حساس لحالة الأحرف.  الموارد “money” ، “Money” و “MONEY” غير متطابقة.  هذه هي 3 موارد منفصلة.
{% endhint %}

{% hint style="info" %}
يمكن للمستخدم واحد أن يكون نفس الدردشة مع بوت.

** على سبيل المثال: ** دردشة خاصة وجماعية.

ولكن في أي مكان لديه ** محاكات ** الموارد
{% endhint %}

## مورد الدردشة

```javascript
let res = Libs.ResourcesLib.chatRes("money");
Bot.sendMessage("Cur your money: " + res.value());
```

{% hint style="info" %}
يمكن لمستخدم واحد الحصول على نفس الدردشات مع bot.

 ** على سبيل المثال **: دردشة خاصة وجماعية.

لكن لديه موارد ** مختلفة ** لكل دردشات
{% endhint %}



## موارد المستخدم والدردشة

 جميع الطرق يمكن أن تكون لموارد المستخدم أو الدردشة.

```javascript
// الحصول على الدقة
let res = Libs.ResourcesLib.userRes("money");
```

`res.name` - اسم الدقة الحالية. فمثلا: 

```javascript
Libs.ResourcesLib.chatRes("BTC").name // is "BTC"
```

## وظائف أساسية

 ### كمية الدقة الحالية

`res.value()` 

### تعيين المبلغ لهذا القرار

`res.set(amount)` 

فمثلا: `Libs.ResourcesLib.userRes("wood").set(10);`

### إضافة مبلغ لهذا القرار

`res.add(amount)` 

### الدقة لديها مثل هذا المبلغ؟

`res.have(amount)`- إذا كانت القيمة res مساوية للمبلغ أو أكثر

### يسلب المبلغ من الموارد

`res.remove(amount)` -  إذا كان لديك ذلك res.removeAnyway\(amount\) - يسلب المبلغ على أي حال.

## الوصول إلى موارد أخرى

### الوصول إلى موارد مستخدم آخر

```javascript
// telegramid - هو معرف telegram لمستخدم آخر
let res = Libs.ResourcesLib.anotherUserRes("money", telegramid);
Bot.sendMessage("Cur your money: " + res.value());
```

### الوصول إلى موارد الدردشة الأخرى

```javascript
// موارد دردشة أخرى
// chatid - إنه معرف برقية للدردشة الأخرى
let res = Libs.ResourcesLib.anotherChatRes("money", chatid);
Bot.sendMessage("Cur your money: " + res.value());
```



## نقل الموارد 

```javascript
let res = Libs.ResourcesLib.userRes("gold");
// telegramid - هو معرف برقية لمستخدم آخر السماح للبرسمات الأخرى = Libs.ResourcesLib.anotherUserRes("gold", telegramid);
```

### إذا كان لديك مورد ...

```javascript
res.takeFromAnother(anotherRes, amount);
res.transferTo(anotherRes, amount)
```

### ...أو على أي حال، حتى الموارد ليست كافية

```javascript
res.takeFromAnotherAnyway(anotherRes, amount)
res.transferToAnyway(anotherRes, amount)
```



### يمكن تبادل موارد مختلفة

على سبيل المثال من "الذهب" ل "الخشب":

`res.exchangeTo(anotherRes, { remove_amount: 10, add_amount:23 } )`

\`\`

## النمو للموارد.

الموارد يمكن أن يكون النمو.

{% hint style="info" %}
على سبيل المثال نمو بسيط:

 ** أضف 5 كل 10 ثوانٍ إلى الدقة **
{% endhint %}

```javascript
let health = Libs.ResourcesLib.userRes("health");
health.set(1);
health.growth.add({value: 5, interval:10 });
```

Interval - انها قيمة في ثوان.  تضاف القيمة كل فاصل

### أضف 5 كل ساعة بقيمة 100 كحد أقصى.

```javascript
//Max value: 100
let secs_in_hour = 1 * 60 * 60;
health.growth.add({
  value: 5,
  interval: secs_in_hour,
  max: 100
});
```

### يمكن أن تكون القيمة سلبية.  إزالة 5 كل 30 ساعة.

```javascript
//Min value: -20
let secs_in_30hours = 1 * 60 * 60 * 30;
health.growth.add({
  value: -5,  // فقط أضف قيمة سالبة
  interval: secs_in_30hours,
  min: -20
});
```



### يمكن أن تحد من الحد الأقصى لعدد التكرار

```javascript
health.growth.add(
   {value: 5,
   interval: secs_in_30hours,
   max_iterations_count: 3
});
```

###يمكن أن النمو بنسبة في المئة.  على سبيل المثال ، أضف 15 ٪ كل شهر مقابل 100 دولار أمريكي

```javascript
let usd = Libs.ResourcesLib.userRes("usd");
usd.set(100);
let secs_in_month = 60 * 60 * 24 * 31;
usd.growth.addPercent({
  value: 15,
  interval: secs_in_month
});
```



### يمكن أن تنمو عن طريق الفائدة المركبة.

 على سبيل المثال ، أضف 0.8٪ يوميًا مقابل 0.5 BTC مع إعادة الاستثمار

```javascript
let btc = Libs.ResourcesLib.userRes("BTC");
btc.set(0.5);
let secs_in_day = 1 * 60 * 60 * 24;
usd.growth.addCompoundInterest({
  value: 0.8,
  interval: secs_in_day
});
```

{% hint style="info" %}
يمكنك الحصول على قيمة دس الأولية من خلال: `res.baseValue()`
{% endhint %}

### طرق أخرى لل res.growth:

`res.growth.info()` - الحصول على معلومات عن النمو الحالي

`res.growth.title()` - الحصول على اللقب.  على سبيل المثال "أضف 5 مرة في 15 ثانية"

`res.growth.isEnabled()` - العودة الحقيقية إذا تم تمكين

`res.growth.stop()` - وقف النمو

`res.growth.progress()` - التقدم الحالي للتكرار المقبل

`res.growth.willCompletedAfter()` - سوف ينتهي من التكرار بعد هذا الوقت في ثوان

### 

### كيفية إضافة النمو إلى موارد أخرى؟

 على سبيل المثال لدينا:

* الودائع المصرفية 100 دولار مع نمو سنوي 10 ٪
 * ومحفظة بسيطة - 500 دولار

كل عام نضيف نمو البنك إلى المحفظة.

#### **Init:** on `/start` command \(أو أي أمر آخر\)

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
wallet.set(500);

let bankDeposit = Libs.ResourcesLib.userRes("deposit");
bankDeposit.set(100);
let secs_in_year = 1 * 60 * 60 * 24 * 365;

bankDeposit.growth.addPercent({
  value: 10,
  interval: secs_in_year
});
```

#### **On** `/wallet` الامر أو الخ

{% hint style="info" %}
يمكننا تشغيل هذا الأمر كل عام.  فمن الممكن على سبيل المثال ، مع
[Auto Retry](https://help.bots.business/commands/auto-retry)

أو يمكن للمستخدم تشغيله يدويًا في أي وقت.
{% endhint %}

```javascript
let wallet = Libs.ResourcesLib.userRes("wallet");
let bankDeposit = Libs.ResourcesLib.userRes("deposit");

// انها قيمة الدقة الأولية
let baseValue = bankDeposit.baseValue();

// إجمالي الدخل بنسبة المئة
let delta = bankDeposit.value() - baseValue;

// إضافة جميع الدخل للمحفظة
wallet.add(delta);
// وإزالته من الودائع المصرفية
bankDeposit.set(baseValue);
```





## How to

### ** س: كيفية إعطاء للإحالة 5 ٪ من إيداع المستخدم الإحالة؟ **

لطفا أنظر 
[https://help.bots.business/libs/refferallib\#how-to](https://help.bots.business/libs/refferallib#how-to)



### ** س: كيفية منح مكافأة لجميع المستخدمين كل يوم؟ **

على سبيل المثال ، أضف 10 إلى رصيد المستخدم كل يوم

Command `/start`

```javascript
let balance = Libs.ResourcesLib.userRes("balance");
balance.set(0);

Bot.run( {
    command: "/addBonus",
    run_after: 1*60*60*24,  // إضافة مكافأة بعد 1 يوم
} )
```

 Command `/addBonus`

```javascript
if(request){
  // لا يمكن للمستخدم تشغيل هذا الأمر يدويا
  Bot.sendMessage("Restricted!")
  return
}

let balance = Libs.ResourcesLib.userRes("balance");
balance.add(10);

// وكرر هذا الأمر مرة أخرى بعد يوم واحد 
Bot.run( {
    command: "/addBonus",
    run_after: 1*60*60*24,  // after one day
} )

Bot.sendMessage("Bonus for you: 10")
```

{% hint style="warning" %}
سيتم تنفيذ الأمر addBonus/ لكل مستخدم.  تنفق 1 التكرار كل يوم لكل مستخدم.

على سبيل المثال ، بالنسبة لـ 100 مستخدم - سيكون 100 تكرار يوميًا.
{% endhint %}



