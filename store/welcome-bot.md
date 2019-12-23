# Welcome bot

هذا الروبوت يحيي جميع الأعضاء الجدد في الدردشة ويقول "صباح الخير" كل يوم.

### صباح الخير كل يوم

استخدم
[Auto Retry](https://help.bots.business/commands/auto-retry)
لهذا.  كل 50 دقيقة نتحقق من الوقت.  إذا كانت الساعة الحالية 6 صباحًا - سيقوم البوت بإرسال رسالة تحية إلى جميع الدردشة.

{% hint style="info" %}
تشغيل إعادة المحاولة التلقائية بشكل دوري.

لدينا 50 دقيقة - لذلك نكرر كل ساعة دون تكرار.
{% endhint %}

تعيين 3000 إلى إعادة المحاولة التلقائية:

![](../.gitbook/assets/image%20%2810%29.png)

####لا يمكن للمستخدمين تشغيل هذا الأمر.

بوت إرسال رسالة إلى جميع الدردشات.  إذا كان المستخدم يشغل هذا الأمر في الساعة 6 صباحًا - فيُرسل رسالة أيضًا!

نحن بحاجة إلى التحقق من إعادة المحاولة التلقائية فقط.  لا تحتوي إعادة المحاولة التلقائية على متغير الدردشة.  توقف التنفيذ في حالة وجود دردشة.

```javascript
//يمكن تشغيلها باستخدام ميزة "إعادة المحاولة التلقائية" فقط!
if(chat){ return }
```

ثم رمز آخر:

```javascript
let time = new Date()
let hours = time.getHours();
let minutes = time.getMinutes();

curTime = "Time: " + hours + ":" + minutes + " GMT-0";
msg = "";

if(hours==6){
  msg = "صباح الخير!\n" + curTime;
}

Bot.sendMessageToAllChats(msg);
```



### تحية كل عضو جديد:

نستخدم الأمر الرئيسي "*/" لهذا الغرض.  انها التقاط جميع الرسائل.

يمكن أن يحتوي الطلب على معلومات حول الأعضاء الجدد:

```javascript
let new_members = request.new_chat_members;
```

نحتاج إلى بناء التحية لجميع المستخدمين:

```javascript
if(new_members.length > 0){
   for(var i=0; i<new_members.length; i++){
      msg = msg + comma + getNameFor(new_members[i])
      comma = ", ";
   }
   Bot.sendMessage(msg);
}
```

يمكن للمستخدم الحصول على اسم المستخدم أو الاسم الأول.  أو لم تفعل:

```javascript
function getNameFor(member){
   let haveAnyNames = member.username&&member.first_name;
   if(!haveAnyNames){ return ""}

   return member.username ? ("@" + member.username) : member.first_name
}
```



#### All code: 

```javascript
let new_members = request.new_chat_members;
let msg = "مرحبا, ";
let comma = "";

function getNameFor(member){
   let haveAnyNames = member.username&&member.first_name;
   if(!haveAnyNames){ return ""}

   return member.username ? ("@" + member.username) : member.first_name
}

if(new_members.length > 0){
   for(var i=0; i<new_members.length; i++){
      msg = msg + comma + getNameFor(new_members[i])
      comma = ", ";
   }
   Bot.sendMessage(msg);
}

if(request.left_chat_member){
  Bot.sendMessage(
    "وداعا, " + getNameFor(request.left_chat_member)
  );
}

```

![](../.gitbook/assets/image%20%2831%29.png)



