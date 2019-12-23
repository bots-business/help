# Inline Bot

لدينا
[Inline bots](https://core.telegram.org/bots/inline) in Telegram:

[![](https://core.telegram.org/file/811140995/1/I-wubuXAnzk/2e39739d0ac6bd5458)](https://core.telegram.org/file/811140995/1/I-wubuXAnzk/2e39739d0ac6bd5458)

> إلى جانب إرسال الأوامر في رسائل أو مجموعات خاصة ، يمكن للمستخدمين التفاعل مع الروبوت الخاص بك عبر 
[**inline queries**](https://core.telegram.org/bots/api#inline-mode).
إذا تم تمكين الاستعلامات المضمّنة ، فيمكن للمستخدمين الاتصال بالروبوت الخاص بك عن طريق كتابة اسم المستخدم الخاص به والاستعلام في **حقل إدخال النص ** في ** أي دردشة**.  يتم إرسال الاستعلام إلى الروبوت الخاص بك في التحديث.  وبهذه الطريقة ، يمكن للناس طلب محتوى من روبوتك في ** أي ** من دردشاتهم أو مجموعاتهم أو قنواتهم دون إرسال أي رسائل على الإطلاق.

{% hint style="warning" %}
لتفعيل هذا الخيار
`/setinline` command to [@BotFather](https://telegram.me/botfather) وقم بتوفير نص العنصر النائب الذي سيراه المستخدم في حقل الإدخال بعد كتابة اسم الروبوت الخاص بك.
{% endhint %}

{% hint style="success" %}
مثال انظر الى البوت [BBHelpBot](https://t.me/bbhelpbot)
{% endhint %}

## الدعم مع BJS - القيادة 
/inlineQuery

تحتاج الى صنع امر
`/inlineQuery`
.هذا الاسم مطلوب

```javascript
// result.query - إنه استعلام من البحث المضمن
if(!request.query){ return }

results = [];
totalResult = 0;

// انها مجموعة من النتائج.
/ / لدينا InlineQueryResultArticle
// core.telegram.org/bots/api#inlinequeryresultarticle
// أنواع أخرى: https://core.telegram.org/bots/api#inlinequeryresult

results.push({
  type: "المقال",
  id: totalResult,
  title: "نص للعنصر",
  input_message_content:
     { "message_text": "هذه الرسالة ستكون في الدردشة" }
})

Api.answerInlineQuery({
  // رؤية حقول أخرى في:
  // core.telegram.org/bots/api#answerinlinequery
  inline_query_id: request.id,
  results: results,
  cache_time: 30000 // cache time in sec
})
```

