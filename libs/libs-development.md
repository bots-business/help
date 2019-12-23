# Libs تطوير

يمكنك إنشاء Lib ​​الخاصة.  الآن من الممكن إنشاء lib فقط مع Git [importing](https://help.bots.business/git/import-bot-from-git-repository).

Official Bots.Business repository available [here](https://github.com/bots-business/store-libs)

يمكنك تخزين الكود الشائع في المكتبة.

{% hint style="success" %}
انظر المكتبات في مكتبة المتجر.  يمكنك نسخ أي مكتبة مجانية وتعديلها.
{% endhint %}

## الأساسي

 على سبيل المثال: رمز في ملف libs
\myLib.js:

```javascript
function hello(){
  Bot.sendMessage("Hello from lib!")
}

function goodbye(name){
  Bot.sendMessage("Goodbye, " + name)
}

publish({
  sayHello: hello,
  sayGoodbyeTo: goodbye     
})
```

ثم يمكنك استخدام Lib في أمر أي bot:

```javascript
Libs.myLib.hello()
Libs.myLib.sayGoodbyeTo("Alice") 
```

## أوامر التقاط

 من الممكن التقاط الأوامر باستخدام lib.

 فمثلا:

 * نوع المستخدم "Hi"
 * بوت الإجابة "مرحبا"

```javascript
function onHiCommand(){
    Bot.sendMessage("Hello");
}

on('Hi', onHiCommand );
```

الأمر الرئيسي " *\" - لالتقاط أي نص من المستخدم مع lib

```javascript
function onMasterCommand(){
    /// إدخال الرمز الخاص بك هنا
}

on('*', onMasterCommand );
```

{% hint style="info" %}
يمكنك استخدام جميع وظائف BJS في Libs
{% endhint %}

## Using HTTP

Lib يمكن أن تؤدي طلبات الويب.  على سبيل المثال: احصل على صفحة من eample.com وأرسل محتواها إلى المستخدم.

```javascript
libPrefix = "myLib"

function load(){
  HTTP.get( {
    url: "http://example.com",
    success: libPrefix + 'onLoading '
    // headers: headers - if you need headers
  } )
}

function onLoading(){
   Bot.sendMessage(content);
}

on(libPrefix + 'onLoading', onLoading );
```

on Bot command:

```javascript
Libs.myLib.load();
```

See [more](https://help.bots.business/scenarios-and-bjs/send-http-request)



