# BlockIOBot

Это демо бот для интеграции с [Block.io](https://block.io). 

### Смотрите также на связанную [библиотеку](https://help.bots.business/libs/blockio)

### С помощью Block.io вы можете:

* Создавать новые кошельки: Bitcoin, Dogecoin, Litecoin
* посмотреть транзакции
* принимать платежи
* сделать вывод
* и др.

### Меню бота

Главное меню на команде /start :

![](../.gitbook/assets/image%20%2833%29.png)

 Меню адресов бота под псевдонимом "Bot addresses":

![](../.gitbook/assets/image%20%2812%29.png)

  
Меню вывода:

![](../.gitbook/assets/image%20%2830%29.png)

Меню инструментов:

![](../.gitbook/assets/image%20%2817%29.png)

### Как это работает?

Бот использует библиотеку BlockIo. 

 Типичный код для команды `/getXXX`:

```java
Libs.BlockIO.Bitcoin.getXXX(
    { onSuccess: "/onGetXXX", onError: "/onerror" }
);
```

`getXXX` - это API методы из [https://block.io/api/simple](https://block.io/api/simple/)

Также мы имеем команды `onSuccess` и `onError`.

Все команды `onSuccess` имеют названия `/onGetXXX`

Бот имеет только одну `onError` команду: `/onerror`:

```javascript
Bot.sendMessage("Ошибка");

if(options&&options.data){
  // в настройках у нас есть сообщение об ошибке от Block.io
  // просто отправь это
  Bot.sendMessage(options.data.error_message);
}
```



#### Например - команда для проверки адреса

Название команды:

```javascript
Libs.BlockIO.Bitcoin.isValidAddress(
  { onSuccess: "/onvalidate",
  onError: "/onerror",
  address: message }
);
```

У нас также есть адрес в переменных сообщения: команда имеет значение «ждать ответа» от пользователя.

Команда: /onvalidate

Мы просто отправляем ответ:

```javascript
//  у нас есть ответ JSON от Block.io в настройках
Bot.sendMessage(inspect(options));
```



### Получение адресов

Команда: `/getMyAddresses`

```javascript
Libs.BlockIO.Bitcoin.getMyAddresses(
  { onSuccess: "/ongetmyadresses", onError: "/onerror" }
);
```

Команда: `"/onGetMyAdresses"`

```javascript
// Block.io отвечает в options (опциях)
let wallets = options;
Bot.sendMessage("Сеть: " + wallets.network);

let addresses = wallets.addresses;
let answer = "*Ваш кошелек:*\n"

let counter = 0;
// у нас есть несколько адресов.
for(let ind in addresses){
  if(counter>10){ break } // не больше 10 адресов

  counter+=1;
  answer= answer + "#️⃣ `" +  addresses[ind].address + "`" +
      "\n  🏷️Метка: `" + 
               addresses[ind].label.split("_").join("") + "`" +
      "\n  💰баланс: `" + 
               addresses[ind].available_balance + "`" +
      "\n  ⏳ожидающий полученный баланс: " + 
               addresses[ind].pending_received_balance +
      "\n  ❌Архив: /archive" + 
               addresses[ind].label +
      "\n\n"
}

Bot.sendMessage(answer);
```

### Транзакции. Входящие и исходящие транзакции

Для исходящих транзакций:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(
    { type: "sent",
     onSuccess: "/onGetOutTransactions", onError: "/onerror" }
);
```

Для входящих транзакций:

```javascript
Libs.BlockIO.Bitcoin.getTransactions(
    { type: "received",
     onSuccess: "/onGetTransactions", onError: "/onerror" }
);
```

Команды `/onGetOutTransactions` и `/onGetTransactions` - одинаковы:

{% code-tabs %}
{% code-tabs-item title="/onGetOutTransactions" %}
```javascript
let transactions = options;
let answer = "";

answer+= "Сеть: " + transactions.network;

function parseOutcoming(tx){
  let sended = tx.amounts_sent;
 
  if(!sended){ return "" }
  let result = ""
  for(let ind in sended){
    result+= "\n  📥получатель: `" + sended[ind].recipient + "`" +
             "\n  💰количество: `" + sended[ind].amount + "`";
  }
  if(result==""){ return "" }
  
  result+="\n  ▪senders: "
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
  answer+= "\n  ⌚время: `" + time + "`";
  answer+= "\n  🔢подтверждения: " + tx.confirmations;
  
  answer+= parseOutcoming(tx)
}

Bot.sendMessage(answer);



```
{% endcode-tabs-item %}

{% code-tabs-item title="/onGetTransactions" %}
```javascript
let transactions = options;
let answer = "";

answer+= "Сеть: " + transactions.network;

function parseIncoming(tx){
  let received = tx.amounts_received;
 
  if(!received){ return "" }
  let result = ""
  for(let ind in received){
    result+= "\n  📥получатель: `" + received[ind].recipient + "`" +
             "\n  💰количество: `" + received[ind].amount + "`";
  }
  if(result==""){ return "" }
  
  result+="\n  ▪senders: "
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
  answer+= "\n  ⌚время: `" + time + "`";
  answer+= "\n  🔢подтверждения: " + tx.confirmations;
  
  answer+= parseIncoming(tx)
}

Bot.sendMessage(answer);



```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Мастер команда "\ *" для адресных действий

Боту нужна команда для архивации адресов.

![](../.gitbook/assets/image%20%2818%29.png)

Нам нужна команда /archiveLabel, где метка это метка для адреса

Следовательно мы имеем мастер команду "\*" с BJS. Это обрабатывает все команды "/archiveXXX":

```javascript
if(message.substring(0, 8)=="/archive"){
   let arr = message.split("/archive");
   let label = arr[1];
   Libs.BlockIO.Bitcoin.archiveAddresses(
      { onSuccess: "/onarchived", onError: "/onerror", labels:label }
   );
}
```





