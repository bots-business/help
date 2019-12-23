---
الوصف: Lib لدعم متعدد اللغات
---

# Lang

## البدء

أولا - تحتاج إلى إعداد اللغات.  على سبيل المثال مع الامر
** /setup **

```javascript
enLang = { user: { whatIsYourName: "What is your name?", }, hello: "Hello!" }
ruLang = { user: { whatIsYourName: "Как тебя зовут?", }, hello: "Привет!" }

// اللغة الأولى هي اللغة الافتراضية
Libs.Lang.setup("en", enLang);
Libs.Lang.setup("ru", ruLang);
```

اللغة الافتراضية الآن هي "الإنجليزية".

 يمكنك استخدام lib الآن:

```javascript
Bot.sendMessage(Libs.Lang.get().hello)
Bot.sendMessage(Libs.Lang.get().user.whatIsYourName)
```

## المهام

 ### تغيير لغة المستخدم

`Libs.Lang.user.setLang("ru")`

{% hint style="info" %}
`يمكنك الحصول على كود لغة المستخدم.  على سبيل المثال في /start:`

```javascript
// in /start command
lang_code = request.from.language_code;
Libs.Lang.user.setLang(lang_code)
```
{% endhint %}

### الحصول على cur طويلة

`var lang = Libs.Lang.user.getLang()`

### تغيير اللغة الافتراضية

```javascript
Libs.Lang.default.setLang("en")
var default = Libs.Lang.default.getCurLang()
```

{% hint style="info" %}
نصائح.  كما يمكنك استخدام قيادة متعددة اللغة

For example: /hello\_en and /hello\_ru
{% endhint %}



**In BJS for command /hello:**

`Bot.runCommand("/hello_" + Libs.Lang.user.getLang())`

{% hint style="success" %}
كما يمكنك استخدام

`Libs.Lang.get("ru")` 

لإجراءات غير المستخدم: في webhooks وغيرها
{% endhint %}



