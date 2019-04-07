# Бот приветствия

Этот бот приветствует всех новых участников в чате и произносит «Доброе утро» каждый день.

### «Доброе утро» каждый день.

Мы используем [Автовоспроизведение](https://help.bots.business/commands/auto-retry) for . Каждые 50 мин. проверяем время. Если текущее время 6 утра - бот отправляет приветственное сообщение во все чаты.

{% hint style="info" %}
Автовоспроизведение запускается периодически.

Мы имеем 50 мин. - поэтому мы повторяем каждый час без повторения.
{% endhint %}

Установить 3000 на автовоспроизведение Retry:

![](../.gitbook/assets/image%20%287%29.png)

#### Пользователи не могут запустить эту команду.

Бот отправляет сообщения во ВСЕ чаты. Если пользователь запусти эту команду в 6 утра - бот тоже отправит сообщение! 

Нам нужна проверка только для автоматической повторной попытки. Автовоспроизведение не имеет чата. Выполнение прерывается, если чат существует.

```javascript
// может быть запущен только с помощью автовоспроизведения!
if(chat){ return }
```

Затем другой код:

```javascript
let time = new Date()
let hours = time.getHours();
let minutes = time.getMinutes();

curTime = "Время: " + hours + ":" + minutes + " GMT-0";
msg = "";

if(hours==6){
  msg = "Доброе утро!\n" + curTime;
}

Bot.sendMessageToAllChats(msg);
```



### Приветствие всех пользователей

Для этого мы используем мастер команду "\*". Это захватит все сообщения .

Запрос может содержать информацию о новых участниках:

```javascript
let new_members = request.new_chat_members;
```

Нам нужны поздравления для всех пользователей:

```javascript
if(new_members.length > 0){
   for(var i=0; i<new_members.length; i++){
      msg = msg + comma + getNameFor(new_members[i])
      comma = ", ";
   }
   Bot.sendMessage(msg);
}
```

Пользователь может иметь юзернейм или имя. Или не иметь:

```javascript
function getNameFor(member){
   let haveAnyNames = member.username&&member.first_name;
   if(!haveAnyNames){ return ""}

   return member.username ? ("@" + member.username) : member.first_name
}
```



#### Все коды: 

```javascript
let new_members = request.new_chat_members;
let msg = "Привет, ";
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
    "Прощай, " + getNameFor(request.left_chat_member)
  );
}

```

![](../.gitbook/assets/image%20%2820%29.png)



