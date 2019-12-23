#دائما تشغيل الأوامر

أحيانًا يكون تنفيذ التعليمات البرمجية مطلوبًا دائمًا.

## Master command

يتم تنفيذ هذا الأمر فقط عند عدم وجود أوامر أخرى.

مجرد استخدام `*` في اسم الأمر.

## أوامر BeforeAll و AfterAll

شفرة هذه الأوامر تنفذ دائما من قبل
\(and after\)
جميع أوامر الأوامر الأخرى.

** مثال. ** تحتاج إلى إضافة تنبيه مهم في جميع الأوامر.  يمكنك إنشاء أمر BeforeAll واحد فقط برمز
`Bot.sendMessage("تنبيه مهم")`

من أجل أمر `BeforeAll` ، استخدم` @ `في اسم الأمر

للأمر `AfterAll` ، استخدم`@@`في اسم الأمر

{% hint style="danger" %}
يرجى الملاحظة.  BJS فقط لأوامر `BeforeAll` و
` AfterAll` 
يتم تشغيله. لا توجد إجابة ولوحة المفاتيح هنا.
{% endhint %}

{% hint style="info" %}
يمكنك مشاركة الدوال والمتغيرات وغيرها باستخدام أوامر "BeforeAll" و "AfterAll".  إنه فعال لأجزاء الكود الشائعة.
{% endhint %}

```javascript
// code for @ BeforeAll command
function myName(){
  return "Peter"
}
```

```javascript
// رمز ل / اختبار الامر
Bot.sendMessage(
  myName()  // result will be "Peter"
)

// يتم تعريف myName في أمر BeforeAll
```

{% hint style="danger" %}
يرجى الملاحظة.  اذا احتجت
`*`, `@`, `@@ كأسماء الأوامر التي يمكنك استخدامها في aliases`
{% endhint %}

## طرق العودة.

`return`
في BeforeAll يعمل الأمر أيضا لجميع الأوامر
إذا كان لديك `return` في أي أمر ، فلن يتم تنفيذ أوامر AfterAll

مثال: عمل نظام الحظر باستخدام BeforeAll

 في الأمر BeforeAll: باسم `@`

```javascript
badUsers = Bot.getProperty("badUsers", { list: {} })

if(badUsers.list[user.telegramid]){
  Bot.sendMessage("You are blocked!");
  return // وهذا يعمل لجميع الاوامر
  // لأنه في الأمر BeforeAll
}
```

In command `/block`:

```javascript
tgID = 1111111;  // أي tgID للحظر. يمكنك تمريرها عبر رسالة مع انتظار الرد
badUsers = Bot.getProperty("badUsers", { list: {} });
badUsers.list[tgID] = true;

// لإلغاء الحظر:
// badUsers.list[tgID] = false;

Bot.setProperty("badUsers", badUsers, "json");

Bot.sendMessage("User with TG id: " + tgID + " banned");

// يمكنك أيضا استخدام كتلة الثابت
// إنه يحفظ التكرار الخاص بك:
// Bot.blockChat (chat.id) ;

// ولكن مع هذا BeforeAll سوف لا تعمل أيضا
```

