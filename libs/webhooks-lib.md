# Webhooks lib

![](../.gitbook/assets/image%20%282%29.png)

يمكن أن يكون التكامل مع الخدمات الخارجية ممكنًا مع إشعارات webhooks.  هذا lib يولد رابط ل webhooks.

## مثال بوت

نرى
[example bot](https://t.me/BBWebhookBot)

{% hint style="info" %}
من عند [wikipedia](https://en.wikipedia.org/wiki/Webhook): a **webhook** is a method of augmenting or altering the behavior of bot, with custom [callbacks](https://en.wikipedia.org/wiki/Callback_%28computer_programming%29).

قد تتم المحافظة على عمليات الاسترجاعات هذه وتعديلها وإدارتها من قِبل المستخدمين والمطورين من الجهات الخارجية الذين قد لا ينتمون بالضرورة إلى موقع الويب أو التطبيق الأصلي.

 صاغ المصطلح "webhook" من قبل جيف ليندساي في عام 2007 من مصطلح برمجة الكمبيوتر [hook](https://en.wikipedia.org/wiki/Hooking).
{% endhint %}

Webhooks هي طريقة أكثر بساطة للتكامل.  تستخدم libs الأخرى أيضًا إشعارات webhooks بالفعل: CoinPayments ، FreeKassa.

## احصل على Webhook Url

```javascript
let webhookUrl = Libs.Webhooks.getUrlFor({
  // سيتم تشغيل هذا الأمر على webhook
  command: "/onWebhook",
  // سيتم تمرير هذا النص إلى الأمر
  content: "هل رأيت القط?",
  // تنفيذ لهذا المستخدم (الحالي)
  user_id: user.id,
  // إعادة توجيه إلى الصفحة مع القط بعد الاتصال webhook
  // تحتاج إلى إزالة هذا للخدمة الخارجية
  redirect_to: "https://cataas.com/cat"
})

```

سيقوم هذا الرمز بإنشاء رابط Webhook.

 بعد تحميل الصفحة عبر هذا الرابط:

 * سيتم تحميل صفحة الويب مع القط
\(شكرا على القط ل)\
[https://cataas.com](https://cataas.com/cat)
* command `/onWebhook`
سيتم تنفيذها على Bot للمستخدم مع user.id
* content "هل رأيت القط?"
سيتم تمرير الأمر
`/onWebhook`

###  أكثر فائدة للخدمات الخارجية

 كقاعدة عامة ، يجب تعيين عنوان URL لـ webhook من لوحة المشرف على الخدمة الخارجية.  لذلك لا يمكننا تعيينه لمستخدم واحد فقط:

```javascript
let webhookUrl = Libs.Webhooks.getUrlFor({
  // سيتم تشغيل هذا الأمر على webhook
  command: "/onWebhook"
})
```

{% hint style="warning" %}
يمكن أن تكون Webhooks باستخدام أساليب GET و POST فقط.  جميع البيانات التي تم تمريرها تحتوي على متغير المحتوى
{% endhint %}

On command `/onWebhook`
يمكننا الحصول على المحتوى المنشور من الخدمة الخارجية service

```javascript
Bot.sendMessage(inspect(content))
// كما يمكنك قراءة البيانات مع Bot.getProperty - تحتاج إلى تخزينها من قبل
```

كقاعدة عامة، يجب على الخدمة الخارجية تمرير بيانات مفيدة حول ويبهوك. على سبيل المثال معلومات عن الدفعات:
 order\_id, user\_id. Use it!

