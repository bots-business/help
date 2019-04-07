# Библиотека

Вы можете хранить общий код в библиотеке.

{% hint style="success" %}
Смотрите библиотеки в магазине библиотек. Вы можете скопировать любую бесплатную библиотеку и пользоваться ею.
{% endhint %}

## Основы

Например: код файле libs\myLib.js:

```javascript
function hello(){
  Bot.sendMessage("Привет от библиотеки!")
}

function goodbye(name){
  Bot.sendMessage("Досвидание, " + name)
}

publish({
  sayHello: hello,
  sayGoodbyeTo: goodbye     
})
```

затем вы можете использовать библиотеку в любой команде бота:

```javascript
Libs.myLib.hello()
Libs.myLib.sayGoodbyeTo("Алиса") 
```

## Запись команд

Можно записывать команды с помощью библиотеки.

Например:

* пользователь пишет "Привет"
* бот отвечает "Привет" 

```javascript
function onHiCommand(){
    Bot.sendMessage("Привет");
}

on('Привет', onHiCommand );
```

Мастер команда "\*" - для записи любого текста пользователя с помощью библиотеки

```javascript
function onMasterCommand(){
    /// запишите сюда ваш код
}

on('*', onMasterCommand );
```

{% hint style="info" %}
Вы можете использовать все функции BJS в библиотеках 
{% endhint %}

## Использование HTTP

Библиотека может выполнять веб запросы. Например: получить страницу из eample.com и отправить ее контент пользователю.

```javascript
libPrefix = "myLib"

function load(){
  HTTP.get( {
    url: "http://example.com",
    success: libPrefix + 'onLoading '
    // headers: headers - if you need headers
  } )
}

function onLoading(){
   Bot.sendMessage(content);
}

on(libPrefix + 'onLoading', onLoading );
```

как это будет выглядеть в команде бота:

```javascript
Libs.myLib.load();
```



