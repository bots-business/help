---
description: Lib for multi language support
---

# Lang

## Getting started

First - need to setup languages. For example with **/setup** command:

```javascript
enLang = { user: { whatIsYourName: "What is your name?", }, hello: "Hello!" }
ruLang = { user: { whatIsYourName: "Как тебя зовут?", }, hello: "Привет!" }

// first language is default language
Libs.Lang.setup("en", enLang);
Libs.Lang.setup("ru", ruLang);
```

Now default language is "english".

You can use lib now:

```javascript
Bot.sendMessage(Libs.Lang.get().hello)
Bot.sendMessage(Libs.Lang.get().user.whatIsYourName)
```

## Functions

### Change user lang

`Libs.Lang.user.setLang("ru")`

{% hint style="info" %}
`You can get user language code. For example in /start:`

```javascript
// in /start command
lang_code = request.from.language_code;
Libs.Lang.user.setLang(lang_code)
```
{% endhint %}

### Get cur lang

`var lang = Libs.Lang.user.getLang()`

### Change default lang

```javascript
Libs.Lang.default.setLang("en")
var default = Libs.Lang.default.getCurLang()
```

{% hint style="info" %}
Tips. Also you can use multi lang command.

For example: /hello\_en and /hello\_ru
{% endhint %}



**In BJS for command /hello:**

`Bot.runCommand("/hello_" + Libs.Lang.user.getLang())`

{% hint style="success" %}
Also you can use 

`Libs.Lang.get("ru")` 

for non user actions: in webhooks and etc
{% endhint %}



