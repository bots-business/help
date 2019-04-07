# Бот помощник

![](../.gitbook/assets/image%20%2813%29.png)

В чате Bots.Business мы много общих вопросов. [Этот бот](https://telegram.me/BBHelpBot) может ответить на них.

Бот имеет только одну команду. Это [Мастер команда](https://help.bots.business/commands#how-to-execute-command-with-any-text-from-user-master-command) - \*.

### Описание кода

Бот получает все сообщения из чата с помощью Мастер команды.

Следовательно нам не нужны иные уведомления о новых участниках чата и т.д.:

```javascript
if(!message){ return }
```

{% hint style="info" %}
У вас есть большой случай для выполнения команды? Вы можете использовать возврат.
{% endhint %}

У бота есть ключевые слова для поиска в сообщениях пользователя. Бот показывает ответ и URL на клавиатуре в сообщении:

```javascript
list = [
   { url: "status.bots.business", keywords: [ 'статус' ],
       answer: 'Похоже вам нужно узнать о возобновлении?' },
   { keywords: [ '/start' ], answer: 'Пожалуйста, не трогайте это здесь' },
   { keywords: [ 'php ', ' php' ], answer: 'PHP? Серьезно? Я люблю только BJS' },
   { keywords: [ 'Привет!', 'Здравствуй' ], answer: 'Хэй!' }
]
```

Также админ может написать что угодно и не нуждаться в помощи. Итак, у нас есть ключ - answerToAdmin:

```javascript
let admin_tg_id = 519829299;
...
{ url: "status.bots.business", keywords: [ 'статус' ], answerToAdmin: true }
```

Иногда нам нужен точный поиск:

```javascript
{ url: "help.bots.business", keywords: [ 'Помощь' ], exact:true}
```

Сообщение может быть в лЮбОмРеГисТРе. Нам это нужно только в нижнем регистре:

```javascript
let stext = message.toLowerCase();
```

Мы используем функции в коде. Так что код проще:

```javascript
// поиск ключевых слов в тексте (stext)
function haveAnyKeyword(item){
  for(var ind in item.keywords){
    // точный поиск
    if(item.exact){
      // точный поиск
      if(stext==item.keywords[ind]){ return true }
      continue;
    }

    if(stext.indexOf(item.keywords[ind])>-1){ return true }
  }
}

// построить ответ
function getAnswerFor(item){
  if((user.telegramid==admin_tg_id)&&(!item.answerToAdmin)){
     // нет никакого ответа для администратора
     return;
  }
  
  let answer = item.answer;
  if(!answer){ answer = "" }

  if(item.url){
    answer = answer + "\nhttp://" + item.url
  }
  return answer;
}

//  просто перебрать весь список ключевых слов
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
Функции делают ваш код более простым. Это хорошо для переписывания и улучшения.
{% endhint %}

Выполнить поиск. И если мы получили ответ - отправить сообщение:

```javascript

let answer = doSearch();
if(answer){
  Bot.sendMessage(answer, {is_reply: true});
}

```

{% hint style="info" %}
В этой команде - у нас есть только одна функция Bot.sendMessage!

Это хорошо - код проще.
{% endhint %}

