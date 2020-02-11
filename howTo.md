# كيف...

## س: ما هو "BJS"؟

إنه بوت JavaScript. انها جافا العادية مع بعض إدراج.

## س: لا أعرف JavaScript. ماذا يجب أن أفعل؟

عادة لا تحتاج إلى شيء معقد لتطوير برامج الروبوت. يمكنك قراءة بضع مقالات حول JS: [https://www.w3schools.com/js/js\_syntax.asp](https://www.w3schools.com/js/js_syntax.asp) or [https://en.wikipedia.org/wiki/JavaScript\_syntax](https://en.wikipedia.org/wiki/JavaScript_syntax)

## س: كيف يمكنني الحصول على إجابتين من أمر واحد؟

يمكنك استخدام إجابة بوت و `Bot.sendMessage("ANY message").`

أو استخدام وظيفتين: رمز BJS:

```javascript
   Bot.sendMessage("ANY message");
   Bot.sendMessage("Other any message");
```

## س: لدي تحذير من markdown للرسالة في الأخطاء. ما هو؟

برقية لها markdawn لتنسيق النص.

عينات:

```text
\n - new line (يسمح بنص متعدد الاسطر)
*bold text*
_ italics _
[text](URL)
`inline fixed-width code`
```text
pre-formatted fixed-width code block
```

لذلك إذا كان لديك markdawn غير صحيح - لديك هذا التحذير. نموذج النص مع matkdawn غير صحيح:

```text
  bot_name
  price 1*
  "user`s" - wrong. Use "user's"!
```

## س: كيف يمكنني استخدام لوحة المفاتيح المضمنة؟

لطفا أنظر
[demo bot](https://telegram.me/DemoInlineKeyboardBot). وهي متوفرة في المتجر.

BJS code:

```javascript
var buttons = [
    {title: "اذهب الي قوقل", url: "https://google.com"},
    {title: "Call command for Button1", command: "/touch 1 زر" },
    {title: "Call command for Button2", command: "/touch 2 زر " }
];

Bot.sendInlineKeyboard(buttons, "يرجى اتخاذ خيار.
بعد ذلك امر آخر 
`/touch` سوف تبدأ مع المعلمات");
```

`buttons` -
انها مجموعة. أنه يحتوي على أزرار. كل زر هو كائن مع
`title` \(required\), `url` or `command`.

يجب أن يكون الزر
`url` or `command`. `url` - أي رابط.

`command` - 
سيتم تنفيذ هذا الأمر بعد الضغط على زر. يمكن أن تحتوي على المعلمات من خلال مسافة.
`Command`
لا يمكن أن يكون أكثر من 64 بايت.

## س: كيفية الروبوت يمكن أن ترسل الرد لرسالة المستخدمين؟

تحتاج إلى تمرير خيارات المعلمة مع `is_reply`

```javascript
  Bot.sendMessage("It is reply message", {is_reply: true} );
```

لأي رسالة في دردشة استخدام: `reply_to_message_id`

```javascript
  Bot.sendMessage("It is reply message", {reply_to_message_id: request.message_id } );
```

كما يمكنك إعادة المحاولة باستخدام لوحة المفاتيح أو المضمنة لوحة المفاتيح أيضا

```javascript
  // إعادة المحاولة للرسالة الأخيرة
  Bot.sendMessage("It is reply message", {is_reply: true } );
  // أو لأي رسالة
  Bot.sendMessage("It is reply message", {reply_to_message_id: request.message_id } );

  Bot.sendInlineKeyboard(
      [ {title: "جوجل", url: "http://google.com" }, {title: "other command", commnad: "/othercommand"} ],
      "يرجى اتخاذ خيار.",
      {reply_to_message_id: request.message_id }
 )
```

يمكنك إعادة المحاولة لأي رسالة من الدردشة.

## س: أنا لا أفهم المتغيرات: 
`request`, `user`, `chat`, and etc.

يمكنك أن ترى ذلك مع
`inspect` وظيفة:

```javascript
  Bot.sendMessage( inspect(request) );
  // أو 
  Bot.inspect(request)   // إذا كان لديك مشكلة مع markdawn
```

## س: أود إنشاء BJS للحد الزمني! مثال في الروبوت ، يمكنك فقط استخدام الأمر كل 24 ساعة!

BJS code:

```javascript
var last_run_at = User.getProperty("last_run_at");
if(last_run_at){
   duration_in_hours = ((new Date) - last_run_at) / 1000/60/60;
}else{
   // وهذه هي المرة الأولى!
   duration_in_hours = 99;
}
if(duration_in_hours>=24){
   User.setProperty("last_run_at", (new Date), "datetime")
   Bot.sendMessage("تقوم بتشغيل الأمر الآن!");
   // أضف كودك
   ...

}else{
   Bot.sendMessage("يرجى العودة لاحقا. ")
}
```

## س: كيف يمكنني إنشاء bjs أنه إذا قمت بالنقر فوق الزر ، فإنه يعيد التوجيه لفتح رابط في الويب؟

لسوء الحظ ، هذا غير معتمد من قبل Telegram API. ولكن يمكنك إرسال رابط إلى الدردشة:
الإجابة:

```text
[Open](http://example.com)
```

## س: كيف يمكنني إنشاء كلمة مرور الوصول إلى الروبوت؟

يمكنك استخدام مجموعة للأوامر. بعد ذلك ، لا يمكن بدء هذه الأوامر إلا من قبل المستخدمين الموجودين في هذه المجموعة. يمكنك تعيين الاستخدام

_مثال_ Bot:

> كلمه السر؟

User:

> 12345

Bot:

> اهلا ,عضو

في هذا المثال ، يجب على المستخدم إدخال كلمة المرور الصحيحة. بعد ذلك ، يتم تعيين المجموعة "الأعضاء" ويمكن للمستخدم تنفيذ جميع أوامر هذه المجموعة. إذا كانت كلمة المرور غير صحيحة ، فستظهر رسالة خطأ

**Bot:** إجابة: `password?` بحاجة إلى\_reply: `true`

BJS code:

```javascript
if(message=="12345"){
  User.addToGroup('Members');
  Bot.sendMessage("Welcome, member!");
}else{
  Bot.sendMessage("Password incorrect");
}
```

## س: هل تعرف رمز BJS الذي يقوم برنامج bot بإرسال رسالة للمستخدم تلقائيًا إذا لم يفعل أي نشاط في الروبوت في وقت معين؟

** 1. يمكنك تخزين آخر وقت نشط ** للمستخدم في خاصية bot:

command `tracking`

> هذا أمر غير مرئي للمستخدمين. يتم تشغيله من الأوامر الأخرى فقط

BJS code:

```javascript
   if(chat.chat_type=="private"){
      // تتبع الدردشات الخاصة فقط

      total_users = Bot.getProperty("total_users");
      if(!total_users){ total_users = 0 }
      Bot.setProperty("total_users", total_users+1, "integer");

      var propLastActiveName = "user" + String(total_users) + "_last_active_at";
      var propChatIdName =  "chat" + String(i) + "_id";

      Bot.setProperty(propLastActiveName, (new Date), "datetime");
      Bot.setProperty(propChatIdName, chat.chatid, "string");  
   }
```

**2. في** _**الاوامر الاخرى**_ **تحتاج الاتصال** `tracking` **أمر** Bjs code:

```javascript
   Bot.runCommand("تتبع");
   // لديك أي رمز هنا
```

> يرجى الملاحظة. هناك حاجة إلى هذا الرمز في جميع أوامر روبوتك.

**3. امر رسالة تلقائية.**
قم بتعيين وقت إعادة المحاولة التلقائي لذلك: 24 ساعة

BJS:

```javascript
   total_users = Bot.getProperty("total_users");
   for(var i=0; i<total_users; i++){
      var propLastActiveName = "user" + String(i) + "_last_active_at";
      var last_active_at = Bot.getProperty(propLastActiveName);
      var duration = (new Date) - last_active_at;
      var duration_in_minutes = duration / 1000 / 60
      if(duration_in_minutes>60*24){
         // غير نشط أكثر من 24 ساعة
         var propChatIdName = "chat" + String(i) + "_id";
         var chatId = Bot.getProperty(propChatIdName);
         Bot.sendMessageToChatWithId(chatId, "Hello! How are you?");
      }
   }
```

## س: هل من الممكن وضع بعض رموز BJS في روبوتنا الذي يخطر المستخدم بسعر العملة المشفرة؟

يمكنك استخدام
[Coinmarketcap API](https://coinmarketcap.com/api/).
على سبيل المثال الصفحة [https://api.coinmarketcap.com/v2/ticker/1/](https://api.coinmarketcap.com/v2/ticker/1/) لديك معلومات حول البيتكوين. نحن بحاجة إلى تحميله مع BJS.

BJS:

```javascript
  ->(https://api.coinmarketcap.com/v2/ticker/1/)
  var result = JSON.parse(content);
  var BTC_USD_Price = result.quotes.USD.price;
  Bot.sendMessage("Current Bitcoin price: " + String(BTC_USD_Price) + " $");
```

## س: هل من الممكن أن يكون لكل زر قيمة؟ كيفية إنشاء مثل على الصورة أدناه:

![](https://i.imgur.com/6bA89pW.png)

يجب تحديث لوحة المفاتيح في كل مرة تتغير فيها القيمة. للقيام بذلك ، أرسل لوحة المفاتيح باستخدام الأمر. على الأرجح ، يجب أن يتم ذلك في العديد من الأوامر.

**BJS**:

```javascript
var balance = User.getProperty("balance"); // أو رمزك هنا
Bot.sendKeyboard(String(balance) + ",\nHelp, Contacts" );
```

{% hint style="info" %}
**استخدم ResLib لأي موارد**
{% endhint %}

#### Command `⚡Balance:`

تحتاج إنشاء القيادة
"⚡Balance:" \(without space\) or `"⚡"` إذا كان لديك مساحة بين "⚡" and "Balance"

**وذلك بعد الضغط على زر:**

* text "⚡Balance: X BTC⚡" سيتم إرسالها إلى الدردشة
* command "Balance:" سيتم تنفيذها
* params "X BTC⚡" سيتم تمريرها إلى BJS

## س: كيفية تعيين الروبوت الذي ينتج عنه ، عندما ينقر أحد أعضاء مجموعة telegram على الأمر bot في telegram bot ، فإن الرسالة الواردة من bot لا ترى إلا من قبل النقرات التي يراها ذاتيا وغير مرئية بواسطة anothe

بوت يمكنه إرسال رسالة إلى هذا المستخدم على انفراد. لذلك يحتاج المستخدم لبدء هذا الروبوت في الدردشة الخاصة في البداية.

ثم في الدردشة الجماعية:

```javascript
Bot.sendMessageToChatWithId(user.telegramid, "اجابة البوت")
```

كما أنه من الممكن إظهار رسالة تنبيه للمستخدم في الدردشة الجماعية مع 
[answerCallbackQuery](https://core.telegram.org/bots/api#answercallbackquery) 
بعد الضغط على زر مضمنة.

## س: ما هو bjs للحصول على عدد الأعضاء الكلي؟

انها ليست "مجموع عدد الأعضاء". يمكنك الحصول على عدد الدردشات. نظرًا لأن مستخدمًا واحدًا يمكن أن يكون له عدة محادثات مع bot: محادثات خاصة وعدة مجموعات.

هذا BJS إرجاع جميع إحصاءات الروبوت:

```javascript
Bot.sendMessage(
  "Total chats: " + bot.statistics.total +
  "\n group chats: " + bot.statistics.group_chats_count +
  "\n super group chats: " + bot.statistics.super_group_chats_count +
  "\n private chats: " + bot.statistics.user_chats_count +
  "\n active during last day chats: " + bot.statistics.active_during_last_day +
  "\n active during last week chats: " + bot.statistics.active_during_last_week +
)
```



## س: كيفية الحصول على موقع من المستخدم؟

يجب على المستخدم إرفاق موقعه للدردشة.

تحتاج القيادة مع
"Wait for answer" خيار
\(أو يمكنك استخدام القيادة ماجستير وقبض على الموقع\)

BJS:

```javascript
// يمكنك فحص جميع البيانات
// Bot.inspect(طلب);

let location = request.location
if(!location){
   Bot.sendMessage("يرجى ارسال الموقع");
   return
}

Bot.sendMessage(
   "Your location is:\n longitude " +
       location.longitude +
       "\n latitude: " +
       location.latitude
 )
```

## س: لدي أمر مع انتظار الرد. كيفية الإلغاء انتظر الرد؟

مثال:
command `/askN،ame` 
انتظر الرد. تحتاج إلى إلغاء ذلك.

إضافة لوحة المفاتيح لهذا الأمر:
keyboard "Cancel" \(كما يمكن أن يكون "Back"\)

BJS:

```javascript
// الخروج على زر إلغاء أو العودة
if((message=="Cancel")||(message=="Back")){
  return // exit
}

// الحصول على اسم هنا
name = message;
Bot.sendMessage("Hello, " + name);

```



## س: كيف تظهر حالة تأهب عند الضغط على زر مضمن؟

BJS:

```javascript
Api.answerCallbackQuery({
  callback_query_id: request.id,
  text: "My alert",
  show_alert: true // أو خطأ - في حالة تأهب في الاعلى
})
```



## س: كيفية التحقق مما إذا كان المستخدم ينضم إلى قناة؟

Command: `/isJoined`

```javascript
chanell = "@MyChanell"

Api.getChatMember({
  chat_id: chanell,
  user_id: user.telegramid,
  on_result :"/onCheckJoin"
})
```

Command `/onCheckJoin`

```javascript
let status = options.result.status;

var isJoined = (
   (status == "member")||
   (status == "administrator")||
   (status == "creator")
)

if(isJoined){
   Bot.sendMessage("أنت عضو شانيل!");
}else{
   Bot.sendMessage("أنت لست عضوًا في chanell!");
}
```

