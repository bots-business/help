# BJS الأمن

BJS
قوية جدا ومرنة.  لكن مع هذا.
ولكن ببساطة اجعل الكود
** ضعيفًا**

{% hint style="danger" %}
يرجى قراءة هذا المقال بعناية

 خاصة إذا كنت تعمل مع المدفوعات ، أرصدة المستخدم ، بيع البضائع من خلال الروبوت
{% endhint %}

## يمكن لأي مستخدم تنفيذ أي أمر

القيادة الضعيفة
`/setBalance`:

```javascript
// يمكن للمشرف إضافة 100 دولار إلى رصيد المستخدمين من خلال telegramid

tgID = params

let res = Libs.ResourcesLib.anotherUserRes("money", tgID);
res.add(100)
Bot.sendMessage("Added 100$ for user");
```

{% hint style="danger" %}
اي مستخدم يمكنة تشغيل
 /setBalance \[telegramid\]
{% endhint %}

لذلك تحتاج التحقق من تنفيذ هذا الأمر فقط للمشرف:

**1.إضافة هذا الأمر إلى** 
[**group**](https://help.bots.business/commands/groups)

![](../.gitbook/assets/image%20%2834%29.png)

إضافة المسؤول إلى المجموعة "admin": 

```javascript
// إنشاء أي أمر مؤقت مع هذا الرمز
// تشغيله للمشرف

// تدمير الامر بعد تنفيذ الامان
// كما يمكنك حماية هذا الأمر بكلمة مرور

User.addToGroup("admin")
```

** 2.  أو يمكنك التحقق من المشرف في BJS **

تحتاج أولا الحصول عليها ADMIN\_TELEGRAM\_ID

```javascript
Bot.sendMessage(user.telegramid)
```

قيادة الأمن:

```javascript
if(user.telegramid!=ADMIN_TELEGRAM_ID){
  return // الخروج من BJS
}

// يمكن للمشرف فقط إضافة 100 دولار إلى رصيد المستخدمين من خلال telegramid

tgID = params

let res = Libs.ResourcesLib.anotherUserRes("money", tgID);
res.add(100)
Bot.sendMessage("أضاف 100 دولار للمستخدم");
```



## يمكن لأي مستخدم تنفيذ أي أمر "SECRET"

على سبيل المثال ، لديك
أمر
`/payment`
\(have "Wait for answer"\)
 مع تنفيذ آخر
"secret" command
`/setBalance` :

```javascript
// أمر /payment

// يوفر المستخدم كلمة مرور oneTime. إذا كانت كلمة المرور صالحة - أضف مكافأة 100 دولار

var oneTimePassword = User.getProperty("oneTimePassword");

if(!oneTimePassword){
  return // ليس لدينا كلمة مرور مرة واحدة الآن
}

if(oneTimePassword=="already taked"){
  // إذا اتخذت بالفعل - موجودة
  return
}

if(oneTimePassword!=message){
  // المستخدم لا يعرف كلمة مرور oneTime
  Bot.sendMessager("Error. Password is wrong")
}

if(oneTimePassword==message){
  // يعرف المستخدم كلمة مرور واحدة
// اجعلها "مأخوذة بالفعل"
  User.setProperty("oneTimePassword", "already taked", "string")
  // run "secret" command
  Bot.runCommand("/setBalance");
  Bot.sendMessage("شكرا لك على الدفع!");
}
```

"Secret" command `/setBalance`

```javascript
let res = Libs.ResourcesLib.userRes("money", tgID);
res.add(100)
Bot.sendMessage("Added 100$ for you");
```

** لذلك يجب على المستخدم: **

 * تشغيل / دفع الأوامر
 * اكتب كلمة المرور السرية لمرة واحدة
* بعد ذلك - سيتم تشغيل الأمر "السري"setBalance/"

** الضعف: يمكن للقراصنة تشغيل
/setBalance 
فقط والحصول على مكافأة على الفور **

تحتاج إلى التحقق من هذا الأمر
`/setBalance`
كان يديرها فقط عن طريق القيادة
`/payment`

إحدى الطرق - تمرير أمر سري في أمر التشغيل كما يلي:

command `/payment`

```javascript
...
// جزء من رمز ل /payment

if(oneTimePassword==message){
  ...
  var secret = "GJHURFVJLHF"
// استخدام السرية الخاصة. يمكنك تخزينها في الممتلكات
  Bot.runCommand("/setBalance");
  Bot.sendMessage("شكرا لك على الدفع");
}
```

{% hint style="danger" %}
لا تستخدم سر "GJHURFVJLHF"!

 إنه ليس عالم سري بالفعل
{% endhint %}

command `/setBalance`

```javascript
if(params=="GJHURFVJLHF"){
   let res = Libs.ResourcesLib.userRes("money", tgID);
   res.add(100)
   Bot.sendMessage("Added 100$ for you");
}else{
   Bot.sendMessage("أنت المتسلل!")
}
```



## التوصيات

### لا تشارك الرمز المميز للبوت الخاص بك ، BB API Key

رمز مميز لـ Bot و BB API Key - هما بيانات الثغرات الأمنية.  لا نشاركهم في أي مكان!



### لا تستخدم أسماء الأوامر الافتراضية
"/onIncome", "/onTransaction" من اجل استيراد الامر

يمكن هاكر القوة الغاشمة هذه أسماء الأوامر ومحاولة تنفيذها

### \*\*\*\*

### **Remove /test command**

إذا كان لديك أي أمر test/ مع BJS غير آمن - إزالته.

يمكن هاكر تنفيذ test/ جدا



### استعمال متغير
###`completed_commands_count`

يمكن لأي شخص تشغيل أي أمر.  ولكن من الممكن جعل الأمر الفرعي المضمون.

 على سبيل المثال القيادة
`/admin`

```javascript
// جعل وصول المسؤول هنا
// ...

Bot.runCommand("/secure")
```

command `/secure`

```javascript
// لا يمكن تشغيل هذا الأمر بواسطة المستخدم
if(completed_commands_count==0){ return }

// فقط عبر Bot.runCommand أو Bot.run أو 
//as "on_result"
// الرمز الآمن الخاص بك هنا
// ...
```

## لا تستخدم أي libs غير الرسمية الآن.

{% hint style="danger" %}
لا تستخدم أي libs غير الرسمية الآن.

 * يمكن لأي lib تشغيل الأمر مع الخيارات.
 * يمكن لأي libs قراءة الخصائص
\(and read your API Keys from other lib\)

ليس لدينا طريقة لحماية هذا الآن.  فقط ** لا تستخدم LIBS الرسمية ** مع CP lib.  حسنًا ، الآن لا توجد مثل هذه المكتبات
{% endhint %}

## سوء الممارسة

Bad BJS:

```javascript
let admin = "Jon Smith";

if (user.first_name==admin){
  // القيام بعمل المشرف هنا
  ...
}
```

{% hint style="danger" %}
يمكن لأي مستخدم تعيين أي
first\_name, last\_name and etc 

يمكن للهاكر تغيير أو إنشاء حساب بهذا الحقل
{% endhint %}



## انظر تقارير BB

[Read info](https://help.bots.business/bb-inspection) حول تقرير BB. تقرير تجريبي لديك توصيات لطيفة. 