---
description: Lib for multi language support
---

# Lang

## Getting started

First - need to setup languages. For example with **/setup** command:

```javascript
var enLang = {
    user: { whatIsYourName: "What is your name?" },
    hello: "Hello!",
    onlyOnEnglish: "Only on english"
    keyboards: {
        mainMenu: { buttons: "Bonus, Friends", text: "menu" }
        bonusMenu: { buttons: "Get bonus, Back", text: "Get your bonus now" }
    }
}

var ruLang = {
    user: { whatIsYourName: "Как тебя зовут?" },
    hello: "Привет!",
    // not translated yet:
    // onlyOnEnglish: "Only on english"
    keyboards: {
        mainMenu: { buttons: "Бонус, Друзья", text: "меню"}
        bonusMenu: { buttons: "Получить бонус, Назад", text: "получи бонус"}
    }
}

// first language is default language
Libs.Lang.setup("en", enLang);  // english is default now
Libs.Lang.setup("ru", ruLang);
```

Now default language is "english".

You can use lib now \(for example, in command `/test`\):

```javascript
var lang = Libs.Lang;
// send messages on default language
Bot.sendMessage(lang.t("hello"))   // Hello!
Bot.sendMessage(lang.t("user.whatIsYourName"))  // What is your name?

Bot.sendMessage(lang.t("onlyOnEnglish")) // "Only on english"
// Default language text always is used
//  for non translated keys yet


// send keyboard for Main menu:
Bot.sendKeyboard(
  lang.t("keyboards.mainMenu.buttons"),
  lang.t("keyboards.mainMenu.text")
)

// or send Bonus menu:
Bot.sendKeyboard(
  lang.t("keyboards.bonusMenu.buttons"),
  lang.t("keyboards.bonusMenu.text")
)
```

## How to use

### Change user lang

`Libs.Lang.user.setLang("ru")`

{% hint style="info" %}
`You can get user language code. For example in /start:`

```javascript
// in /start command
var lang_code = request.from.language_code;
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

### Get translation by language code

Sometimes we do not have user object. For example, on income [webhooks](webhooks-lib.md) or with Bot.runAll command and etc. Therefore, we cannot determine the current language.

The following code might be helpful in such cases.

```javascript
// command /test
// you can pass language code in params
var lng = params;
// or in options with Bot.run(command: "/test", options: {lngCode: "fr"} )
var lng = options.lngCode

// So now:
// lng = "fr"

Bot.sendMessage(
    Libs.Lang.t("hello", lng)
)   // Привет!
```



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
var enLang = {
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
var cmdName;

for(var i in languages){
    cmdName = "lng-" + languages[i].code;
    Bot.run({ command: cmdName })
}

Bot.sendMessage("Multi Languages - installed");
```

### Make a simple command based structure

```javascript
var enLang = {
    command1: {
      text: "your text",
      // keys
      ...
      }
      ...
    }
    command2: {
      text: "your text"
      // keys,
      ...
    }
    // bot menus
    menus: {
      mainMenu: {
        keyboard: 
        text: 
      },
      helpMenu: {
        keyboad:
        text: 
      }
    },
    // common links
    links: {
      homePage: "<a href='example.com'>Home page</a>",
    },
    alert: {
      error: "sorry, we have error"
    }
    ...
}

Libs.Lang.setup("en", enLang);
```

### Make translation for text and keyboard:

```javascript
var enLang = {
    command1: {
       text: "Please confirm",
       keyboard: "Yes, Cancel",
       
       // you can also attach Inline keyboard:
       inlineKeyboard: {
         buttons: [ 
           {title: "google", url: "http://google.com" },
           {title: "other command", command: "/othercommand"}
          ],
          text: "Please make a choice."
        }
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

// send inline keyboard:
Bot.sendInlineKeyboard(
   Libs.Lang.t("command1.inlineKeyboard.buttons"),
   Libs.Lang.t("command1.inlineKeyboard.text"),
)
```

### Use aliases translations

See [this](https://help.bots.business/libs/lang#using-aliases)



