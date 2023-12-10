# SmartBot



## Introduction

`SmartBot` is a versatile tool designed to enhance bot interaction and management, particularly for multi-language support. This guide focuses on setting up and initializing `SmartBot` for your projects, ensuring a smooth start for beginners in programming.



{% hint style="info" %}
We have bot demo: [BBDemoTaskBot](https://t.me/BBDemoTaskBot) - free available in the Store.&#x20;
{% endhint %}

## Setup

You need setup Lang File



## Creating an Instance of SmartBot

To use `SmartBot`, you first need to create an instance. This is typically done in the main bot file or where you handle your bot's logic.

#### Syntax:

```javascript
let smartBot = new SmartBot(options);
```

#### Options:

* `params`: Initial parameters for the bot.
* `defaultMarkdown`: The default formatting style for messages (e.g., Markdown, HTML).
* skip\_cmd\_folders: Don't process commands in this folders (it can be "Setup", "Admin" folders for example)
* strict\_params: Default - false. If true - error will be thrown if param not found but it is needed in Command's.
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

Prefer to use after all command "@@":

```javascript
// command "@@"
smartBot.handle();
```

{% hint style="success" %}
You can make "return" in command and "@@"-command will be not run. It can be helpful in some case.
{% endhint %}



## Debugging

If `debug` is set to `true`, `SmartBot` will provide detailed error messages, which is helpful for troubleshooting and ensuring your bot behaves as expected.



## Best Practices

* **Keep Language Files Updated**: Regularly update your language files to reflect changes in your bot's functionality.
* **Test Thoroughly**: Always test your bot for various scenarios, especially after adding new features or making changes.
* **Handle Errors Gracefully**: Ensure that your bot handles errors smoothly and provides helpful feedback to users.



## Conclusion

Setting up and initializing `SmartBot` is a straightforward process. By carefully configuring your language files and utilizing the robust features of `SmartBot`, you can create an interactive and user-friendly bot experience. Remember to test extensively and update your language files as your bot evolves.
