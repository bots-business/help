---
description: Библиотека для поддержки мультиязычности
---

# Язык

## Начало

Сначала - нужно настроить языки. Например, с помощью команды / setup:

```javascript
enLang = { user: { whatIsYourName: "What is your name?", }, hello: "Hello!" }
ruLang = { user: { whatIsYourName: "Как тебя зовут?", }, hello: "Привет!" }

// начальный язык стоит по умолчанию
Libs.Lang.setup("en", enLang);
Libs.Lang.setup("ru", ruLang);
```

Теперь языком по умолчанию является «английский».

Вы можете использовать библиотеку сейчас:

```javascript
Bot.sendMessage(Libs.Lang.get().hello)
Bot.sendMessage(Libs.Lang.get().user.whatIsYourName)
```

## Функции

### Изменить язык пользователя

`Libs.Lang.user.setLang("ru")`

### Получить недавний язык

`var lang = Libs.Lang.user.getLang()`

### Изменить язык по умолчанию

```javascript
Libs.Lang.default.setLang("en")
var default = Libs.Lang.default.getCurLang()
```

{% hint style="info" %}
Подсказки. Также вы можете использовать мултиязычные команды.

Например: / hello \ _en и / hello \ _ru
{% endhint %}



**Команда для /hello в BJS:**

`Bot.runCommand("/hello_" + Libs.Lang.user.getLang())`

{% hint style="success" %}
Также вы можете использовать

`Libs.Lang.get("ru")` 

не для пользовательских действий: в webhooks(вебхуках) и т.д.
{% endhint %}



