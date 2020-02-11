# رسالة البث والتحرير



<table>
  <thead>
    <tr>
      <th style="text-align:left">الوظيفة</th>
      <th style="text-align:left">الوصف</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessage(text)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى الدردشة الحالية</p>
        <p></p>
        <p><code>Bot.sendMessage ("مرحبا من البوت")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChatWithId(chatid, النص)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة للدردشة مع معرف.  ويرد chatid الحالي للدردشة في
          chat.chatid</p>
        <p></p>
        <p><code>Bot.sendMessageToChatWithId ("45445454521" ، "مرحبًا بالمستخدمين!")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChat (chat_name ، نص)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى الدردشة الأخرى.  يجب تثبيت الروبوت في دردشة أخرى</p>
        <p></p>
        <p><code>sendMessageToChat ("OtherTestChat" ، "مرحبا بالجميع!")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllPrivateChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى جميع دردشات خاصة</p>
        <p></p>
        <p><code>Bot.sendMessageToAllPrivateChats ("مرحبًا بالمستخدم")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى جميع دردشات المجموعة</p>
        <p></p>
        <p><code>Bot.sendMessageToAllGroupChats ("هذه دردشة جماعية")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllSuperGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى جميع دردشة مجموعة سوبر</p>
        <p></p>
        <p><code>Bot.sendMessageToAllSuperGroupChats ("أنت في مجموعة رائعة!")</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال رسالة إلى جميع الدردشة</p>
        <p></p>
        <p><code>Bot.sendMessageToAllChats ("هذه الرسالة لجميع المستخدمين")</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>**الأمان**

{% hint style="danger" %}
يرجى ملاحظة أنه يمكن للمستخدم إنشاء دردشة مع أي اسم وإضافة إليها الروبوت الخاص بك.

لذلك ، وظيفة
`Bot.sendMessageToChatWithId`
هو الأفضل من وظيفة
`Bot.sendMessageToChat`.
{% endhint %}

## ** تحرير الرسائل **

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>الوظيفة</b>
      </th>
      <th style="text-align:left">الوصف</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">editMessage(value, message_id, options)</td>
      <td style="text-align:left">
        <p>تحرير الرسالة ذات القيمة و message_id</p>
        <p></p>
        <p><code>Bot.editMessage(&quot;new text&quot;, 20)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">editMessageInChat(chat_id, value, message_id)</td>
      <td style="text-align:left">
        <p>تحرير الرسالة ذات القيمة و message_id في الدردشة </p>
        <p></p>
        <p><code>Bot.editMessageInChat(10512154, &quot;new text&quot;, 25)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
message\_id - إنه مُعرّف فريد لكل المحادثات في هذا الروبوت.
{% endhint %}

### **Message\_id لرسائل الدخل للبوت **

لرسائل الدخل للبوت: استخدم
 `request.message_id`

#### مثال

```javascript
let msg_id = request.message_id;
Bot.editMessage("new text", msg_id)

// also you can store message_id for future editing:
User.setProperty("msg_id", msg_id, "integer");
// or if you want to edit message from others chats:
Bot.setProperty("msg_id" + chat.chatid, msg_id, "integer");
```

{% hint style="warning" %}
Message\_id - لها قيمة فريدة لجميع الأحاديث من بوت.  لذلك لدينا واحد فقط
message\_id
مع القيمة "2" وفقط في دردشة واحدة.
{% endhint %}

### **Message\_id for** رسالة من البوت

استخدم `on_result` في الخيارات - انظر [this](https://help.bots.business/scenarios-and-bjs/message-broadcasting#options-for-sendmessage-editmessage-and-sendkeyboard-functionals-reply-disable-notification-disable-web-page-preview)

#### مثال

في الأمر الأول:

```javascript
Bot.sendMessage("hello",
   {on_result: "onMessageSending" }
)
```

في القيادة `onMessageSending`:

```javascript
// يمكنك فحص جميع النتائج:
// Bot.inspect (خيارات)

let msg_id = options.result.message_id;
Bot.editMessage("new text", msg_id);

// كما يمكنك حفظ message_id للمستقبل:
// Bot.setProperty ("MSG-for-edit" + chat.chatid، msg_id، "integer")؛
```



### ** خيارات لوظائف sendMessage و editMessage و
sendKeyboard: الرد ، تعطيل الإخطار ، تعطيل معاينة صفحة الويب **

يمكنك تمرير خيارات المعلمة إلى أي
`sendMessageXXX`, `sendKeyboard`, `editMesage`, `editMessageInChat` و`sendInlineKeyboard` وظيفة:

```javascript
  let options = { disable_notification: true, reply_to_message_id: request.message_id };
  Bot.sendMessage("Hello from bot", options);
  Bot.sendMessageToChatWithId("45445454521", "Hello users!", options);
  Bot.sendKeyboard("about, help,\ncontacts", "send keyboard now", options)
```

| المعلمة | اكتب | الوصف | | : --- | : --- | : --- | | disable\_web\_page\_preview | منطقية | تعطيل معاينات الرابط للروابط في هذه الرسالة | | disable\_notification | منطقية | يرسل الرسالة بصمت. سيتلقى المستخدمون إشعارًا بدون صوت. | | reply\_to\_message\_id | عدد صحيح | إذا كانت الرسالة ردًا ، فقم بمعرف الرسالة الأصلية | | is\_reply | منطقية | إذا كانت الرسالة ردًا على الرسالة السابقة | | parse\_mode | خيط | أرسل `Markdawn` أو `HTML` ، إذا كنت تريد أن تعرض تطبيقات Telegram نصًا غامقًا أو مائلًا أو ثابتًا أو عناوين URL مضمّنة في رسالة الروبوت الخاصة بك. الافتراضي `تخفيض السعر. يمكن أن يكون `Markdawn` أو `HTML` أو `لا شيء` | | result\_to\_bot\_property | خيط | فمن الأفضل للاستخدام "on\_result". تخزين نتيجة للرسالة إرسال خاصية bot بهذا الاسم. يمكنك قراءة هذه النتيجة لاحقًا في أوامر أخرى بواسطة Bot.getProperty | | on\_result | خيط | استدعاء هذا الأمر بعد الطريقة مع النتيجة | \`\`