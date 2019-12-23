# BlockIOBot

هذا الروبوت هو تجريبي للتكامل مع
 [Block.io](https://block.io). 

###انظر أيضا ذات الصلة [Lib](https://help.bots.business/libs/blockio)

### مع Block.io يمكنك:

 * إنشاء محافظ جديدة: بيتكوين ، Dogecoin ، Litecoin
 * تبدو المعاملات
 * قبول المدفوعات
 * جعلها تنسحب
 * الخ

### بوت القوائم

القائمة الرئيسية start/ في امر:

![](../.gitbook/assets/image%20%2852%29.png)

قائمة عناوين بوت في الاسم المستعار "عناوين بوت":

![](../.gitbook/assets/image%20%2815%29.png)

  
قائمة السحب:

![](../.gitbook/assets/image%20%2848%29.png)

قائمة الأدوات:

![](../.gitbook/assets/image%20%2827%29.png)

### كيف يعمل؟

يستخدم بوت BlockIo lib. 

رمز نموذجي للقيادة وهو
`/getXXX` :

```java
Libs.BlockIO.Bitcoin.getXXX(
    { onSuccess: "/onGetXXX", onError: "/onerror" }
);
```

`getXXX` - من اساليب apk فمن [https://block.io/api/simple](https://block.io/api/simple/)

أيضا لدينا أوامر `onSucc.ess` و `on.Error`.

All `onSuccess` قيادة لها اسم`/onGetXXX`

بوت لديها واحد فقط
`onError` command: `/onerror`:

```javascript
Bot.sendMessage("Error");

if(options&&options.data){
  // في الخيارات لدينا رسالة خطأ من Block.io
  // فقط ارسلها
  Bot.sendMessage(options.data.error_message);
}
```



#### على سبيل المثال - أمر للتحقق من صحة العنوان

 اسم الأمر هو:

```javascript
Libs.BlockIO.Bitcoin.isValidAddress(
  { onSuccess: "/onvalidate",
  onError: "/onerror",
  address: message }
);
```

لدينا أيضًا عنوان في متغيرات الرسالة: الامر لها قيمة "انتظر الإجابة" من المستخدم.

الامر:
/onvalidate

نحن فقط نرسل الرد:

```javascript
// لدينا استجابة json من Block.io في الخيارات 
Bot.sendMessage(inspect(options));
```



### الحصول على العناوين

أمر:
`/getMyAddresses`

```javascript
Libs.BlockIO.Bitcoin.getMyAddresses(
  { onSuccess: "/ongetmyadresses", onError: "/onerror" }
);
```

أمر:
`"/onGetMyAdresses"`

```javascript
// استجابة Block.io في الخيارات
واسمح للمحافظ = الخيارات ؛
Bot.sendMessage("Network: " + wallets.network);

let addresses = wallets.addresses;
let answer = "*محافظك:*\n"

let counter = 0;
// لدينا عدة عناوين.
for(let ind in addresses){
  if(counter>10){ break } // no more then 10 addresses

  counter+=1;
  answer= answer + "#锔忊儯 `" +  addresses[ind].address + "`" +
      "\n  馃彿锔廘abel: `" + 
               addresses[ind].label.split("_").join("") + "`" +
      "\n  馃挵balance: `" + 
               addresses[ind].available_balance + "`" +
      "\n  鈴硃ending received balance: " + 
               addresses[ind].pending_received_balance +
      "\n  鉂孉rchive: /archive" + 
               addresses[ind].label +
      "\n\n"
}

Bot.sendMessage(answer);
```

### المعاملات.  الدخل والمعاملات الصادرة

للمعاملات الصادرة:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(
    { type: "sent",
     onSuccess: "/onGetOutTransactions", onError: "/onerror" }
);
```

لمعاملات الدخل:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(
    { type: "received",
     onSuccess: "/onGetTransactions", onError: "/onerror" }
);
```

`/onGetOutTransactions` and `/onGetTransactions` الأمر - مشابه:

{% tabs %}
{% tab title="/onGetOutTransactions" %}
```javascript
let transactions = options;
let answer = "";

answer+= "Network: " + transactions.network;

function parseOutcoming(tx){
  let sended = tx.amounts_sent;
 
  if(!sended){ return "" }
  let result = ""
  for(let ind in sended){
    result+= "\n  馃摜recipient: `" + sended[ind].recipient + "`" +
             "\n  馃挵amount: `" + sended[ind].amount + "`";
  }
  if(result==""){ return "" }
  
  result+="\n  鈻猻enders: "
  for(let ind in tx.senders){
     result+= "`" + tx.senders[ind] + "` ";
  }
  
  return result;
}

let tx, time;
for(let ind in transactions.txs){
  tx = transactions.txs[ind];
  time = new Date(tx.time*1000);
  time = time.toLocaleString()
  
  answer+= "\n\nTXID:`" + tx.txid + "`";
  answer+= "\n  鈱歵ime: `" + time + "`";
  answer+= "\n  馃敘confirmations: " + tx.confirmations;
  
  answer+= parseOutcoming(tx)
}

Bot.sendMessage(answer);



```
{% endtab %}

{% tab title="/onGetTransactions" %}
```javascript
let transactions = options;
let answer = "";

answer+= "Network: " + transactions.network;

function parseIncoming(tx){
  let received = tx.amounts_received;
 
  if(!received){ return "" }
  let result = ""
  for(let ind in received){
    result+= "\n  馃摜recipient: `" + received[ind].recipient + "`" +
             "\n  馃挵amount: `" + received[ind].amount + "`";
  }
  if(result==""){ return "" }
  
  result+="\n  鈻猻enders: "
  for(let ind in tx.senders){
     result+= "`" + tx.senders[ind] + "` ";
  }
  
  return result;
}

let tx, time;
for(let ind in transactions.txs){
  tx = transactions.txs[ind];
  time = new Date(tx.time*1000);
  time = time.toLocaleString()
  
  answer+= "\n\nTXID:`" + tx.txid + "`";
  answer+= "\n  鈱歵ime: `" + time + "`";
  answer+= "\n  馃敘confirmations: " + tx.confirmations;
  
  answer+= parseIncoming(tx)
}

Bot.sendMessage(answer);



```
{% endtab %}
{% endtabs %}



### الأمر الرئيسي "*\" لإجراءات العنوان
 بوت بحاجة إلى الأمر لأرشفة العناوين.

![](../.gitbook/assets/image%20%2828%29.png)

نحتاج القيادة
/archiveLabel, حيث التسمية تسمية للعنوان

لذلك لدينا القيادة الرئيسية "*\" مع BJS. يقوم بمعالجة جميع أوامر
"/ archiveXXX":

```javascript
if(message.substring(0, 8)=="/archive"){
   let arr = message.split("/archive");
   let label = arr[1];
   Libs.BlockIO.Bitcoin.archiveAddresses(
      { onSuccess: "/onarchived", onError: "/onerror", labels:label }
   );
}
```





