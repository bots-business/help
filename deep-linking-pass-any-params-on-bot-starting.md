# الربط العميق - تمرير أي معايير على عدم البدء

يمكنك تمرير أي معلمات على bot تبدأ بـ BJS مع الرابط:

[http://t.me/BOT\_NAME?start=PARAMS](http://t.me/BOT_NAME?start=PARAMS)

In BJS:

```javascript
Bot.sendMessage(params);
```

فمثلا:

[http://t.me/botsbusinessadminbot?start=API\_TOKEN](http://t.me/botsbusinessadminbot?start=API_TOKEN)

إننا نمرر رمز واجهة برمجة تطبيقات المستخدم إلى الروبوت.

```javascript
let api_token = params;
// افعل أي شيء تريده مع هذه المعلمات
Bot.sendMessage("Your api token is: " + api_token)
```

