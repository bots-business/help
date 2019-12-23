# Auto Retry \(AR\)

يمكن تشغيل الأمر بشكل دوري.  فمثلا:
* bot send [message "Hello" ](https://help.bots.business/store/welcome-bot#good-morning-every-day)
كل 24 ساعة
* قم بتنزيل صفحة الويب كل 1 ساعة وتحليلها.  انظر لدينا [PlayMarketNewsBot](https://telegram.me/PlayMarketNewsBot)

{% hint style="danger" %}
إعادة المحاولة التلقائية قضى 1 التكرار على كل شوط.  وبالتالي ، إذا وضعت AR لمرة واحدة في الدقيقة
\(60 secs\), سيكون 1440 التكرار في اليوم الواحد
{% endhint %}

لذلك يجب تعيين وقت إعادة المحاولة التلقائي:

* for 10 minutes: 60\*10 = **600** secs
* for 1 hour: 60\*60 = **3600** secs
* for 24 hours: 60\*60\*24 = **86400** secs.
* for 1 year: 86400 \* 365 = **24966000** secs

#### تعديل إعادة المحاولة التلقائية في التطبيق عند تحرير الأوامر:

![Auto retry time can be modified on command editing](../.gitbook/assets/image%20%2853%29.png)



{% hint style="warning" %}
إعادة المحاولة التلقائية تعمل فقط مع [BJS](https://help.bots.business/scenarios-and-bjs). There are now [Variables](https://help.bots.business/scenarios-and-bjs/variables): chat, user, request.
{% endhint %}

### التعامل معها فقط على BJS!

نظرًا لأن إعادة المحاولة التلقائية التي تمت تهيئتها تلقائيًا ، لا توجد محادثة حالية أو مستخدم أو طلب

لذلك لا يمكننا استخدام:

```javascript
Bot.sendMessage("Hello");  //لا يعمل مع إعادة المحاولة التلقائية
```

لماذا ا؟  بوت لا أعرف ماهي الدردشة لإرسال رسالة!  لا توجد دردشة حالية.

لذلك تحتاج إلى تعريف الدردشة:

```javascript
Bot.sendMessage({text: "Hello", chat_id: YOUR_CHAT_ID});
```

#### كيف يمكنني معرفة معرف الدردشة؟

إنشاء أمر بسيط
\(without Auto Reply\): `/chat` with BJS:

```javascript
Bot.sendMessage(chat.chatid);
```

وقم بتشغيله على تلك الدردشة حيث تحتاج إلى "إعادة المحاولة التلقائية" لاحقًا.  هذا الأمر يعود YOUR\_CHAT\_ID

املأها في الأمر السابق باستخدام "إعادة المحاولة التلقائية".



{% hint style="info" %}
يمكنك استخدام Bot.getProperty و Bot.setProperty.  لذلك يمكنك حفظ
 chat\_id
في أمر واحد ومن ثم الحصول عليه على أمر إعادة المحاولة التلقائية
{% endhint %}

{% hint style="warning" %}
لا يمكنك استخدام User.getProperty و User.setProperty.

 لا يوجد مستخدم على إعادة المحاولة التلقائية!
{% endhint %}









