---
description: Lib for multi language support
---

# Lang

## Getting started

First - need to setup languages. For example with **/setup** command:

```javascript
enLang = {
    user: { whatIsYourName: "What is your name?" },
    hello: "Hello!",
    onlyOnEnglish: "Only on english"
}

ruLang = {
    user: { whatIsYourName: "Как тебя зовут?" },
    hello: "Привет!"
    // not translated yet:
    // onlyOnEnglish: "Only on english"
}

// first language is default language
Libs.Lang.setup("en", enLang);
Libs.Lang.setup("ru", ruLang);
```

Now default language is "english".

You can use lib now:

```javascript
Bot.sendMessage(Libs.Lang.t("hello"))   // Hello!
Bot.sendMessage(Libs.Lang.t("user.whatIsYourName"))  // What is your name?

Bot.sendMessage(Libs.Lang.t("hello", "ru"))   // Привет!

Bot.sendMessage(Libs.Lang.t("onlyOnEnglish")) // "Only on english"
// Default language text is used
//  for non translated keys yet

Bot.sendMessage(Libs.Lang.t("onlyOnEnglish", "ru")) // "Only on english"
```



## How to use

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

`Libs.Lang.t("translation-key", "ru")` 

for non user actions: in webhooks and etc
{% endhint %}



### Get all language json

```javascript
// get all json for default language:
Bot.sendMessage(Libs.Lang.get())

// get all json for "ru":
Bot.sendMessage(Libs.Lang.get("ru"))
```



### Using aliases

You can set aliases for language.

For `/setup`

```javascript
enLang = {
    aliases: {
       "home, dashboard, /main": "/start",
       "alias1,alias2": "/otherCommand"
    }
    user: { whatIsYourName: "What is your name?" },
}

Libs.Lang.setup("en", enLang);
```

For [Master command](https://help.bots.business/scenarios-and-bjs/always-running-commands#master-command) \* :

```javascript
// find multilanguage aliases
var cmd = Libs.Lang.getCommandByAlias(message);
if(cmd){ Bot.run({ command: cmd }) }

// also it is possible define language
// var cmd = Libs.Lang.getCommandByAlias(message, "ru");
// if(cmd){ Bot.run({ command: cmd }) }
```

 

## Good practices

### **Use different language files** \(command\) for each language.

For example "lng-en.js", "lng-fr.js" and etc. So it's easier to translate. 

In `/setup` you can make something like this:

```javascript
var languages = ["en", "es", "cn", "ru"];
for(var i in languages){
    cmdName = "lng-" + languages[i].code;
    Bot.run({ command: cmdName })
}

Bot.sendMessage("Multi Languages - installed");
```

### Make a simple command based structure

```javascript
enLang = {
    command1: {
      // keys
    }
    command2: {
      // keys
    }
    //...
}

Libs.Lang.setup("en", enLang);
```

### Make translation for text and keyboard:

```javascript
enLang = {
    command1: {
       text: "Please confirm",
       keyboard: "Yes, Cancel" 
    }
    //...
}

Libs.Lang.setup("en", enLang);
```

So in `/command1` you can make:

```javascript
Bot.sendKeyboard(
   Libs.Lang.t("command1.keyboard"),
   Libs.Lang.t("command1.text"),
)
```

### Use aliases translations

See [this](https://help.bots.business/libs/lang#using-aliases)



