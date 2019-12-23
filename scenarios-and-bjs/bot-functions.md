# وظائف بوت

<table>
  <thead>
    <tr>
      <th style="text-align:left">الوظيفة</th>
      <th style="text-align:left">الوصف</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.runCommand(command, options)</code>
      </td>
      <td style="text-align:left">
        <p>Run other command</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;)</code>
        </p>
        <p>
          <br />and with options:</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;, {phone: &quot;+15424&quot;, email: &quot;example@example.com&quot;})</code>
        </p>
        <p>
          <br />in second command /contact:</p>
        <p>Bot.sendMessage(&quot;Phone is:&quot; + options.phone);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.run(options)</code>
      </td>
      <td style="text-align:left">
        <p>Run other command</p>
        <p>Bot.run({ command: &quot;/contact&quot; })</p>
        <p></p>
        <p><a href="https://help.bots.business/scenarios-and-bjs/bot-functions#bot-run-options">see more</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.clearRunAfter(options)</code>
      </td>
      <td style="text-align:left">
        <p>امسح الأمر الآخر باستخدام run_after حسب التسمية</p>
        <p></p>
        <p><a href="https://help.bots.business/scenarios-and-bjs/bot-functions#bot-clearrunafter-options">see more</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال لوحة المفاتيح والرسالة.  الرسالة مطلوبة</p>
        <p></p>
        <p><code>Bot.sendKeyboard(&quot;about, help,\ncontacts&quot;, &quot;send keyboard now&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>إرسال لوحة المفاتيح والرسالة المأهولة. رسالة مطلوبة. أزرار هي صفيف. يجب أن يكون الزر حقول نصية: العنوان (مطلوب) url  أمر او.</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;other command&quot;, command: &quot;/othercommand&quot;} ], &quot;Please make a choice.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboardToChatWithId(chat_id, buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>أرسل لوحة مفاتيح مضمّنة ورسالة للدردشة مع chat_id</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard(&apos;852378745487&apos;, [ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;other command&quot;, command: &quot;/othercommand&quot;} ], &quot;Please make a choice.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons)</code>
      </td>
      <td style="text-align:left">
        <p>تحرير موجود لوحة المفاتيح المضمنة بعد تنفيذ الأمر الذي تم استدعاؤه
           بواسطة الزر الخاص به</p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ])</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons, message_id, chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>تحرير موجود لوحة المفاتيح المضمنة للرسالة مع message_id في الدردشة مع
           chat_id.  إذا كانت chat_id فارغة ، فسيتم استخدام الدردشة الحالية</p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ], request.message.message_id, chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>Set property with name for bot</p>
        <p></p>
        <p><code>Bot.setProperty(&quot;TotalScore&quot;, 100, &quot;integer&quot;)</code> 
        </p>
        <p></p>
        <p>يمكن أن يكون النوع عددًا صحيحًا أو عائمًا أو نصًا json أو وقت</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.getProperty(name)</code>
      </td>
      <td style="text-align:left">
        <p>قراءة الممتلكات مع الاسم.  الاسم حساس لحالة الأحرف.  الاسم حساس لحالة الأحرف.</p>
        <p></p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;)</code>
        </p>
        <p></p>
        <p>يمكن الحصول على خاصية ذات قيمة افتراضية لخاصية غير موجودة:</p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;, 100)</code> 
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.importCSV()</code>
      </td>
      <td style="text-align:left">CSV import. More info <a href="https://help.bots.business/create-bot-from-google-table">here</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.blockChat(chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Block chat:</p>
        <p><code>Bot.blockChat(chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.unblockChat(chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Unblock chat:</p>
        <p><code>Bot.unblockChat(chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.inspect(value)</code>
      </td>
      <td style="text-align:left">إرسال قيمة تفتيش للدردشة.  جيد لتصحيح</td>
    </tr>
  </tbody>
</table>

** الوصول إلى الممتلكات ** في الإجابة:

> يمكنك أيضًا استخدام الخصائص في إجابة الأمر.  على سبيل المثال ، يمكنك القيام بذلك باستخدام الأمر
مجموع الدرجات:
/hello
`<TotalScore>!`

in BJS:

> ويمكنك استخدامها في `Bot.sendMessage("<TotalScore>")`



## Bot.run\(خيارات\)

تشغيل قيادة أخرى

```javascript
Bot.run(params)
```

| مجال | الوصف |
| :--- | :--- |
| `command` | **مطلوب**. قيادة للتشغيل. على سبيل المثال "/ بدء". يمكن أن يمر params |
| `options` | json لتمرير الأمر. متاح من خلال الخيارات في هذا الأمر |
| `run_after` | التأخير بالثواني قبل الأمر callName حساس لحالة الأحرف. |
| `user_id` | user\_id لتمرير. ** بشكل افتراضي ** هذا هو user.id الحالي
| `chat_id` | chat\_id لتمرير. ** افتراضيا ** هذا هو chat.id الحالي |
| `label` | يمكن استخدامها للتطهير باستخدام `Bot.clearRunAfter` |

**مثال 1**
قم بتشغيل أمر آخر
`/balance` مع تأخير 1 ساعة للمستخدم الحالي

```javascript
Bot.run( {
    command: "/balance",
    run_after: 1*60*60,  // 1 ساعة تأخير
        // label: "runBalance"  // التسمية يمكن استخدامها لإزالة الدعوة في المستقبل
} )
```

** مثال 2 **. 
قم بتشغيل أمر آخر `balance/` مع تأخير لمدة 5 أيام لهذا المستخدم

```javascript
Bot.run( {
    command: "/balance",
    run_after: 60*60*24*5,  // 5 days delay
    // خيارات: { كمية: 5, عملة: "BTC" } 
// يمكنك تمرير البيانات
    chat_id: chat.id  // أو استخدم chat_id آخر
    user_id: user.id  // أو استخدام user.id آخر
} )
```

{% hint style="danger" %}
لا يمكنك استخدام chat.chatid و user.telegramid مع طريقة Bot.run.

فقط chat.id or user.id
{% endhint %}

{% hint style="success" %}
تخزين chat.id آخر أو user.id إلى propertis إذا لم تتمكن من تمريره على الفور.
{% endhint %}

## Bot.clearRunAfter\(options\)

يمكن مسح الأمر المستقبل \ (s \) التنفيذ
استقر بها Bot.run

{% hint style="info" %}
استخدم هذه الوظيفة إذا لم تكن هناك حاجة إلى استدعاء الأمر في المستقبل
{% endhint %}

```javascript
// احذف جميع أوامر الإعدام المستقبلية
Bot.clearRunAfter()
```

```javascript
// حذف جميع أوامر تنفيذ الأوامر المستقبلية مع التصنيف "myLabel"
Bot.clearRunAfter({ label: "myLabel"})
```

| مجال | الوصف |
| :--- | :--- |
| `label` | **مطلوب**. أمر للمقاصة. على سبيل المثال "myLabel" |

**مثال 1**. 
قم بتشغيل أمر آخر
`/work` مع تأخير 5 أيام. وأزل هذا التأخير
 \ (على سبيل المثال في اليوم الثالث \)

```javascript
Bot.run( {
    command: "/balance",
    run_after: 60*60*24*5, 
// 5 أيام تأخير التصنيف: "myLabel"
} )
```

في اليوم الثالث ، علمنا أن المكالمة لم تعد مطلوبة:

```javascript
// قم بإزالة جميع عمليات الإعدام المستقبلية باستخدام التصنيف "mylabel"
Bot.clearRunAfter( {
    label: "myLabel"
} )
```



