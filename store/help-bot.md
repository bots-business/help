# بوت المساعدة

![](../.gitbook/assets/image%20%2816%29.png)

في دردشة Bots.Business لدينا العديد من الأسئلة العامة. 
[This bot](https://telegram.me/BBHelpBot) 
يمكن الإجابة عن الموضوع.

بوت لديه امر واحدة فقط. هذا الأمر هو
[Master command](https://help.bots.business/commands#how-to-execute-command-with-any-text-from-user-master-command) - \*.

### وصف الكود

بوت تلقي جميع الرسائل من الدردشة مع الأمر Master.

 لذلك نحن لسنا بحاجة إلى أي إشعارات حول أعضاء الدردشة الجدد وغيرها:

```javascript
if(!message){ return }
```

{% hint style="info" %}
هل لديك حالة كبيرة لتنفيذ الأمر؟ يمكنك استخدام العودة.
{% endhint %}

بوت لديه كلمات رئيسية للبحث في رسائل المستخدم.  لا تظهر الإجابة وعنوان url للكلمة الرئيسية في الرسالة:

```javascript
list = [
   { url: "status.bots.business", keywords: [ 'status' ],
       answer: 'Seems do you need to know uptime status?' },
   { keywords: [ '/start' ], answer: 'Please do not touch it here' },
   { keywords: [ 'php ', ' php' ], answer: 'PHP? Really? I love BJS only' },
   { keywords: [ 'hi!', 'hello' ], answer: 'Hey!' }
]
```

أيضا المشرف يمكن أن يكتب أي شيء ولا تحتاج إلى أي مساعدة.  لذلك لدينا مفتاح - answerToAdmin:

```javascript
let admin_tg_id = 519829299;
...
{ url: "status.bots.business", keywords: [ 'status' ], answerToAdmin: true }
```

في بعض الأحيان نحتاج إلى البحث الدقيق:

```javascript
{ url: "help.bots.business", keywords: [ 'help' ], exact:true}
```

يمكن أن تكون الرسالة في aNyCAse. نحتاجها فقط في الأحرف الصغيرة:

```javascript
let stext = message.toLowerCase();
```

نستخدم وظائف في الكود  لذلك الرمز أكثر بساطة:

```javascript
// البحث عن الكلمات الرئيسية في الرسالة (stext)
function haveAnyKeyword(item){
  for(var ind in item.keywords){
    // البحث الدقيق
    if(item.exact){
      // البحث الدقيق
      if(stext==item.keywords[ind]){ return true }
      continue;
    }

    if(stext.indexOf(item.keywords[ind])>-1){ return true }
  }
}

// بناء الجواب
function getAnswerFor(item){
  if((user.telegramid==admin_tg_id)&&(!item.answerToAdmin)){
     // لا توجد إجابة للمشرف
     return;
  }
  
  let answer = item.answer;
  if(!answer){ answer = "" }

  if(item.url){
    answer = answer + "\nhttp://" + item.url
  }
  return answer;
}

// مجرد  قائمة نصفيه جميع قائمة الكلمات الرئيسية 
function doSearch(){
  let item;
  let answer;

  for(var ind in list){
    item = list[ind];
    if(haveAnyKeyword(item.keywords)){
      return getAnswerFor(item);
    }
  }
}
```

{% hint style="info" %}
وظائف خلق رمز أكثر بساطة.  انه لامر جيد لإعادة كتابة والتحسين.
{% endhint %}

إجراء البحث.  وإذا حصلنا على إجابة - أرسل رسالة:

```javascript

let answer = doSearch();
if(answer){
  Bot.sendMessage(answer, {is_reply: true});
}

```

{% hint style="info" %}
في هذا الأمر - لدينا دالة Bot.sendMessage واحدة فقط!

انها جيدة - رمز أكثر بساطة.
{% endhint %}

