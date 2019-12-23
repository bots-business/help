# هيكل مستودع

{% hint style="success" %}
يمكنك جعل التصدير لأي روبوت مجاني في المتجر. فقط تثبيته. 

انه لامر جيد للممارسة.
{% endhint %}

### bot.json ملف

لطفا أنظر [this](https://help.bots.business/git/file-bot-json)

### الأوامر - في مجلد الأوامر

اسم الملف - هو اسم الأمر 
\(ولكن يمكن إعادة كتابته في وصف الأمر\)

{% hint style="warning" %}
للأوامر مع
"/" \(for example command "/start"\) file name is "\_start"
{% endhint %}

القيادة يمكن أن يكون:
`name`, `help`, `aliases` \(second names\), `answer`, `keyboard`, `scnarios` \(for simple logic\) وغيرها من الخيارات.

{% hint style="success" %}
إذا كان الأمر يحتوي على مجلد - فهو موجود في مجلد على القرص بنفس الاسم
{% endhint %}

#### وصف القيادة

إنه رأس ملف اختياري:

```javascript
/*CMD
  command: /test
  help: this is help for ccommand
  need_reply: [ true or false here ]
  auto_retry_time: [ time in sec ]
  folder: ملفي
  answer: إنه مثال للإجابة
/test command
  keyboard: button1, button2
  aliases: /test2, /test3
CMD*/
```

{% hint style="info" %}
وصف الأوامر - حظر اختياري.
{% endhint %}

متعدد الخطوط المدعومة أيضا. على سبيل المثال للإجابة:

```javascript
/*CMD
    <<ANSWER
test answer
with several
lines
  ANSWER
CMD*/
```

{% hint style="info" %}
يمكنك الحصول على الجواب فقط
\(or others\) المفتاح في وصف الأمر. جميع المفاتيح - الخيارات
{% endhint %}

See [more](https://help.bots.business/commands)

#### هيئة القيادة

إنه رمز الأمر في JavaScript. استخدم Bot Java Script للمنطق المنطقي في الأمر.

فمثلا:

`Bot.sendMessage(2+2);`

See [more](https://help.bots.business/scenarios-and-bjs)



### المكتبات - في مجلد libs

يمكنك تخزين الكود الشائع في مجلد libs

See [more](https://help.bots.business/git/library)



