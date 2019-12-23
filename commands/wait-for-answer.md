# Wait for answer

## ما هو "انتظر الإجابة"؟

إنها بحاجة إلى علامة `Wait for answerer` إذا احتجت إلى استجابة من المستخدم.

![Can be modified on command editing](../.gitbook/assets/image%20%286%29.png)

مثال على تنفيذ أمر واحد:

Bot:

> ما اسمك؟

User:

> جون

Bot:

> مرحبا ، جون

command:

```javascript
answer: ما اسمك؟
need_reply: true
BJS: Bot.sendMessage( "مرحبا, " + الرسالة );
```

لذا يُنفّذ رمز BJS فقط بعد إجابة المستخدم

## كيفية إلغاء "انتظر"؟

مثال على إلغاء الأمر مع "انتظر":

Bot:

> ما اسمك؟

استخدم \(press "❌ Back" on keybord\):

> ❌ Back

BJS:

```javascript
if(message=="❌ Back"){
   return  // "الخروج من الأمر على "العودة
}

Bot.sendMessage( " مرحبا, " + الرسالة );
```

أو يمكنك تشغيل 
/menu command على "Back"

```javascript
if(message=="❌ Back"){
   Bot.runCommand("/menu")
   return // "الخروج من الأمر على "العودة
}

Bot.sendMessage( " مرحبا, " + الرسالة );
```



