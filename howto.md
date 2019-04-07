# Кодирование в BJS. Как...

## В: Что такое "BJS"?
Это Bot JavaScript. Это обычный Java с некоторыми новшествами.

## В: Я не знаю JavaScript. Что мне делать?
Обычно, для создания ботов, ничего сложного или сверхестественного не нужно.
Вы можете прочесть пару статей о JS:
https://www.w3schools.com/js/js_syntax.asp или https://en.wikipedia.org/wiki/JavaScript_syntax

## В: Как получить два ответа с помощью одной команды?
Вы одновременно можете использовать bot answer и `Bot.sendMessage("ЛЮБОЙ текст").`

или две функции:
BJS код:
```js
   Bot.sendMessage("ЛЮБОЕ сообщение");
   Bot.sendMessage("Следущее сообщение");
```

## В: У меня Markdown предупреждение на сообщение в отделе "Ошибки". Что это значит?
Telegram использует markdown для форматирования текста.

образцы:
```
\n - абзац (многострочный текст также допускается)
*Жирный текст*
_ кривой _
[текст](ссылка)
`встроенный код фиксированной ширины`
```текст
предварительно отформатированный кодовый блок фиксированной ширины

```

Если у вас неправильное markdown форматирование - у вас эти предупреждения.
Образцы с неправильным форматированием:
```
  назвние_бота
  цена 1*
  "user`s" - неправильно. Печатайте "user's"!
```

## В: Как использовать inline клавиатуру?
Пожалуйста смотрите в [демо боте](https://telegram.me/DemoInlineKeyboardBot). Он доступен в магазине.

BJS код:
```js
var buttons = [
    {title: "Go to Google", url: "https://google.com"},
    {title: "Call command for Button1", command: "/touch Button1" },
    {title: "Call command for Button2", command: "/touch Button2" }
];

Bot.sendInlineKeyboard(buttons, "Please make a choice. After that, another command `/touch` will be started with parameters");
```

`buttons` - это массив. Содержит кнопки. Каждая кнопка это объект с `title` ("название"; объязательно), `url`("ссылка") или `command`("команда").

Кнопка должна иметь `url`(ссылку) или `command`(команду).
`url` - любая ссылка.

`command` - это команда будет произведена после нажатия на кнопку. Может иметь param (параметр) через пробел. `Command` (команда) не может превышать 64 битов.

## В: Как бот может отвечать на сообщения пользователей?
Вам нужно будет воспользоваться параметром `is_reply`

```js
  Bot.sendMessage("Этот ответ на сообщение", {is_reply: true} );
```

Чтобы ответить на любое сообщение в чате, используйте: `reply_to_message_id`

```js
  Bot.sendMessage("Ответ на сообщение", {reply_to_message_id: request.message_id } );
```


А также вы можете воспользоваться, как обычными, так и inline кнопками
```js
  // ответ на последнее сообщение
  Bot.sendMessage("Ответ на сообщение", {is_reply: true } );
  // или же на любое другое
  Bot.sendMessage("Ответ на сообщение", {reply_to_message_id: request.message_id } );
  
  Bot.sendInlineKeyboard(
      [ {title: "google", url: "http://google.com" }, {title: "other command", commnad: "/othercommand"} ],
      "Please make a choice.",
      {reply_to_message_id: request.message_id }
 )
```
Вы можете попробовать с любым другим сообщением из чата.

## В: Я не понял переменные: `request`, `user`, `chat`, и т.д.
Вы можете посмотреть с помощью фунции`inspect`:
```js
  Bot.sendMessage( inspect(request) );
```


## В: Я хотел бы создать bjs с временным ограничением! В примере, команду можно использовать только каждые 24 часа!
BJS код:
```js
var last_run_at = User.getProperty("last_run_at");
if(last_run_at){
   duration_in_hours = ((new Date) - last_run_at) / 1000/60/60;
}else{
   // It is the first time!
   duration_in_hours = 99;
}
if(duration_in_hours>=24){
   User.setProperty("last_run_at", (new Date), "datetime")
   Bot.sendMessage("You run command now!");
   // add your code
   ...
   
}else{
   Bot.sendMessage("Пожалуйста вернитесь позже. ")
}
```

## В: Как создать BJS, где, при нажатии на кнопку, открывается ссылка в веб странице?
К сожалению, Telegram API не поддерживает эту функцию.
Но вы можете просто отправить ссылку:
answer:
```
[Открыть](http://example.com)
```

## В: Как создать пароль для доступа к боту?
Вы можете воспользоваться групповым разделением для команды. Эти команды могут быть воспроизведены только теми, кто находится в определенной группе. Вы можете назначать пользователям определенную группу, создав BJS с использованием верификации с паролем.

*Пример*
Бот:
> пароль?

Пользователь:
> 12345

Бот:
> Добро пожаловать, member!

Здесь показано, что юзер должен ввести пароль. После чего, зачисляется в группу `Members` и дальше может использовать команды этой группы. Если пароль окажется неверным, то появится сообщение об ошибке.

**Бот:**
answer: `пароль?`
need_reply: `true`

BJS код:
```js
if(data.message=="12345"){
  User.addToGroup('Members');
  Bot.sendMessage("Добро пожаловать, member!");
}else{
  Bot.sendMessage("Пароль неверный");
}
```

## В: А вы знали что бот может авмтоматически написать юзерам, если те не были активны в определенное время?

**1. Вы можете записать Последнее Активное Время** пользозателей в property(база данных) бота :

команда `отслеживание`
> Это невидимая команда для пользователя. Запускается вместе с другой командой

BJS код:
```js
   if(data.chat.chat_type=="private"){
      // track only private chats
   
      total_users = Bot.getProperty("total_users");
      if(!total_users){ total_users = 0 }
      Bot.setProperty("total_users", total_users+1, "integer");

      var propLastActiveName = "user" + String(total_users) + "_last_active_at";
      var propChatIdName =  "chat" + String(i) + "_id";

      Bot.setProperty(propLastActiveName, (new Date), "datetime");
      Bot.setProperty(propChatIdName, data.chat.chatid, "string");  
   }
```

**2. Вам нужно всего лишь запустить `отслеживание` вместе с *другими командами*.**
Bjs код:
```js
   Bot.runCommand("tracking");
   // ваш код
```

> Помните. Этот код нужен в каждой команде вашего бота.

**3. Команда автоматического сообщения.**
Установите для него время автоматического повтора: 24 часа

BJS:
```js
   total_users = Bot.getProperty("total_users");
   for(var i=0; i<total_users; i++){
      var propLastActiveName = "user" + String(i) + "_last_active_at";
      var last_active_at = Bot.getProperty(propLastActiveName);
      var duration = (new Date) - last_active_at;
      var duration_in_minutes = duration / 1000 / 60
      if(duration_in_minutes>60*24){
         // not active more than 24 hours
         var propChatIdName = "chat" + String(i) + "_id";
         var chatId = Bot.getProperty(propChatIdName);
         Bot.sendMessageToChatWithId(chatId, "Привет! Как ты?");
      }
   }
   
```

## В: Возможно ли создать BJS код в нашем боте, который оповещает юзеров о ценах криптовалют?
Вы можете использовать [Coinmarketcap API](https://coinmarketcap.com/api/). 
Напримен эта страница https://api.coinmarketcap.com/v2/ticker/1/ имеет информацию о Биткоине.
Вам нужно всего лишь загругить этот BJS.

BJS:
```js
  ->(https://api.coinmarketcap.com/v2/ticker/1/)
  var result = JSON.parse(data.content);
  var BTC_USD_Price = result.data.quotes.USD.price;
  Bot.sendMessage("Недавняя цена Bitcoin : " + String(BTC_USD_Price) + " $");
```

## В: Может ли кнопка иметь переменную? Как создать кнопку похожую на фото:
![](https://i.imgur.com/6bA89pW.png)

Вы должны, каждый раз, обновлять кнопку, чтобы увидеть изменения переменной. Для этого, отправьте кнопку с командой. Лучше всего совершать это в нескольких командах.

BJS:
```js
var balance = User.getProperty("balance"); // или ваш код здесь
Bot.sendKeyboard(String(balance) + ",\nПомощь, Контакты" );
```

**Важно**
> Вы не можете установить команду на эту кнопку!
