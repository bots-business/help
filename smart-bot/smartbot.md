# SmartBot

## Introduction

`SmartBot` is a versatile tool designed to enhance bot interaction and management, particularly for multi-language support. This guide focuses on setting up and initializing `SmartBot` for your projects, ensuring a smooth start for beginners in programming.

{% hint style="info" %}
We have bot demo: [BBDemoTaskBot](https://t.me/BBDemoTaskBot) - free available in the Store.&#x20;
{% endhint %}

## Setup

You need setup Lang File. Please read [here](lang-file.md)



## Creating an Instance of SmartBot

To use `SmartBot`, you first need to create an instance. This is typically done in the main bot file or where you handle your bot's logic.

#### Syntax:

```javascript
let smartBot = new SmartBot(options);
```

{% hint style="success" %}
You need to put this code:

* in [BeforeAll](../bjs/always-running-commands.md#beforeall-and-afterall-commands) command - "@"
* and create blank [Master Command](../bjs/always-running-commands.md#master-command) (because we need to track all commands we need blank "\*" command)
{% endhint %}

#### Options:

* `params`: Initial parameters for the bot.
* `defaultMarkdown`: The default formatting style for messages (e.g., Markdown, HTML).
* skip\_cmd\_folders: Don't process commands in this folders (it can be "Setup", "Admin" folders for example)
* strict\_params: Default - false. If true - error will be thrown if param not found but it is needed in Command's. It is good for debugging.
* `debug`: A boolean flag for enabling debugging. Default: false.

#### Example:

```javascript
let smartBot = new SmartBot({
  params: {
    balance: 0,
    name: user.first_name
  }
});
```

## Handling Commands

`SmartBot` manages commands based on the language file. Use the `handle` method to process incoming commands and generate appropriate responses.

#### Example:

Prefer to use after all [command "@@"](../bjs/always-running-commands.md#beforeall-and-afterall-commands):

```javascript
// command "@@"
smartBot.handle();
```

{% hint style="success" %}
You can make "return" in command and[ "@@"-command ](../bjs/always-running-commands.md#beforeall-and-afterall-commands)will be not run. It can be helpful in some case.
{% endhint %}



### Adding Params

One of `SmartBot`'s key features is its ability to handle dynamic content through variables.



Use the `add` method to include dynamic content in responses:

```javascript
smartBot.add({ username: user.name, balance: user.balance });

// or it can be "set" method
// but with this method all another props will be deleted
// smartBot.set({ username: user.name, balance: user.balance });
```

So we can use username and balance props for command in [Lang File](lang-file.md) now:

```json
...
// "/balance" commands
"/balance": {
   text: "Hello, {username}. Your balance: {balance}"
}
...
```



{% hint style="success" %}
Params from options after SmartBot.run (or Bot.run) - are added automatically
{% endhint %}



#### Running other Command

Use the `run` method to execute another command within a command:

```javascript
smartBot.run({
  command: "/anotherCommand"
});

// if you want edit mode:
smartBot.run({
  command: "/anotherCommand edit"
});
```

{% hint style="success" %}
It is same method like Bot.run but current options will be passed automatically
{% endhint %}

### is Alias method

```javascript
let isAlias = smartBot.isAlias(message);

if(isAlias){
  // for example we can have alias "Cancel", "Exit" on button
  // so we just make exit from command
  return
}

// other code
```



### Filling Content

The `fill` method replaces all vars like "{data}" in text with actual variable values:

```javascript
let response = smartBot.fill("Hello, {username}, your balance is {balance}");
```

{% hint style="info" %}
As a rule, there is no need to use this method - everything should happen automatically
{% endhint %}

## Change language for user

You can change language for user via this code:

```javascript
// change user language to "en"
smartBot.setUserLang("en");
// or to "fr"
smartBot.setUserLang("fr");

// get current user language:
let lngCode = smartBot.getUserLang(); // it will be "fr" here
```

You can define translation. Read about this [here](lang-file.md).

## Debugging

If `debug` is set to `true`, `SmartBot` will provide detailed error messages, which is helpful for troubleshooting and ensuring your bot behaves as expected.



## Best Practices

* **Keep Language Files Updated**: Regularly update your language files to reflect changes in your bot's functionality.
* **Test Thoroughly**: Always test your bot for various scenarios, especially after adding new features or making changes.
* **Handle Errors Gracefully**: Ensure that your bot handles errors smoothly and provides helpful feedback to users.



## Conclusion

Setting up and initializing `SmartBot` is a straightforward process. By carefully configuring your language files and utilizing the robust features of `SmartBot`, you can create an interactive and user-friendly bot experience. Remember to test extensively and update your language files as your bot evolves.
