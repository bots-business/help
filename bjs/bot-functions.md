# Bot functions

| Function                                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Bot.runCommand(command, options)`                              | <p>Run other command</p><p><code>Bot.runCommand("/contact")</code></p><p><br>and with options: </p><p><code>Bot.runCommand("/contact", {phone: "+15424", email: "example@example.com"})</code></p><p><br>in second command /contact:</p><p>Bot.sendMessage("Phone is:" + options.phone);</p>                                                                                                                |
| `Bot.run(options)`                                              | <p>Run other command</p><p>Bot.run({ command: "/contact" })</p><p></p><p><a href="bot-functions.md#bot-run-params">see more</a></p>                                                                                                                                                                                                                                                                         |
| `Bot.clearRunAfter(options)`                                    | <p>Clear other command with run_after by label </p><p></p><p><a href="bot-functions.md#bot-clearrunafter-options">see more</a></p>                                                                                                                                                                                                                                                                          |
| `Bot.runAll(options)`                                           | <p>Run other command for all chats</p><p><code>Bot.runAll({ command: "/broadcast" })</code></p><p></p><p><a href="bot-functions.md#bot-runall-options">see more</a></p>                                                                                                                                                                                                                                     |
| `Bot.sendKeyboard(buttons, message)`                            | <p>send keyboard and message. Message is required</p><p></p><p><code>Bot.sendKeyboard("about, help,\ncontacts", "send keyboard now")</code></p>                                                                                                                                                                                                                                                             |
| `Bot.sendInlineKeyboard(buttons, message)`                      | <p>Send inline keyboard and message. Message is required. Buttons is array. Button must have text fields: title(required), url or command.</p><p></p><p><code>Bot.sendInlineKeyboard([ {title: "google", url: "http://google.com" }, {title: "other command", command: "/othercommand"} ], "Please make a choice.")</code></p>                                                                              |
| `Bot.sendInlineKeyboardToChatWithId(chat_id, buttons, message)` | <p>Send inline keyboard and message to chat with chat_id</p><p></p><p><code>Bot.sendInlineKeyboard('852378745487', [ {title: "google", url: "http://google.com" }, {title: "other command", command: "/othercommand"} ], "Please make a choice.")</code></p>                                                                                                                                                |
| `Bot.editInlineKeyboard(buttons)`                               | <p>Edit exist inline keyboard after executing the command that was called by its button</p><p></p><p><code>Bot.editInlineKeyboard([ {title: "google", url: "http://google.com" } ])</code></p>                                                                                                                                                                                                              |
| `Bot.editInlineKeyboard(buttons, message_id, chat_id)`          | <p>Edit exist inline keyboard for message with message_id in the chat with chat_id. If chat_id is blank current chat is used</p><p></p><p><code>Bot.editInlineKeyboard([ {title: "google", url: "http://google.com" } ], request.message.message_id, chat.id)</code></p>                                                                                                                                    |
| `Bot.setProperty(name, value, type)`                            | <p>Set property with name for bot</p><p></p><p><code>Bot.setProperty("TotalScore", 100, "integer")</code> </p><p></p><p>Type can be: <code>integer, float, string, text, json, datetime</code></p>                                                                                                                                                                                                          |
| `Bot.getProperty(name)`                                         | <p>Read property with name. Name is case sensitive. Name is case sensitive.</p><p></p><p><code>Bot.getProperty("TotalScore")</code></p><p></p><p>can get property with default value for non exist property:</p><p><code>Bot.getProperty("TotalScore", 100)</code> </p><p><br>can get property of another bot:<br><code>Bot.getProperty({ name: "propName", other_bot_id: OTHER_BOT_ID })</code></p><p></p> |
| `Bot.importCSV()`                                               | CSV import. More info [here](https://help.bots.business/create-bot-from-google-table)                                                                                                                                                                                                                                                                                                                       |
| `Bot.blockChat(chat_id)`                                        | <p>Block chat:</p><p><code>Bot.blockChat(chat.id)</code></p>                                                                                                                                                                                                                                                                                                                                                |
| `Bot.unblockChat(chat_id)`                                      | <p>Unblock chat:</p><p><code>Bot.unblockChat(chat.id)</code></p>                                                                                                                                                                                                                                                                                                                                            |
| `Bot.inspect(value)`                                            | Send inspected value to chat. Good for debug                                                                                                                                                                                                                                                                                                                                                                |



**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Total score: <TotalScore>!`

in BJS:

> And you can use it in `Bot.sendMessage("<TotalScore>")`



## Bot.run(params)

Run other command

```javascript
Bot.run(params)
```

| Field       | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| `command`   | **Required**. Command for run. For example "/start". Can pass params   |
| `options`   | json for passing to command. Available through options in this command |
| `run_after` | delay in seconds before command callingName is case sensitive.         |
| `bot_id`    | bot_id for passing. **By default** this is current bot.id              |
| `user_id`   | user_id for passing. **By default** this is current user.id            |
| `chat_id`   | chat_id for passing. **By default** this is current chat.id            |
| `label`     | can be used for clearing with `Bot.clearRunAfter`                      |

**Example 1**. Run another command `/balance` with delay 1 hour for current user

```javascript
Bot.run( {
    command: "/balance",
    run_after: 1*60*60,  // 1 hour delay
    // label: "runBalance"  // label can be used for remove future calling
} )
```

**Example 2**. Run another command `/balance` with delay 5 days for this user

```javascript
Bot.run( {
    command: "/balance",
    run_after: 60*60*24*5,  // 5 days delay
    // options: { amount: 5, currency: "BTC" }  // you can pass data
    chat_id: chat.id  // or use another chat_id
    user_id: user.id  // or use another user.id
    // bot_id: ANOTHER_BOT_ID // to run command for your another bot 
} )
```

{% hint style="danger" %}
You can not use chat.chatid and user.telegramid with Bot.run method.

Only chat.id or user.id
{% endhint %}

{% hint style="success" %}
Store another chat.id or user.id to propertis if you can not pass it imeditally. 
{% endhint %}

## Bot.clearRunAfter(options)

Can clear future command(s) execution setted by Bot.run

{% hint style="info" %}
Use this function if future command calling not needed already
{% endhint %}

```javascript
// delete all future commands executions
Bot.clearRunAfter()
```

```javascript
// delete all future commands executions with label "myLabel"
Bot.clearRunAfter({ label: "myLabel"})
```

| Field   | Description                                                |
| ------- | ---------------------------------------------------------- |
| `label` | **Required**. Command for clearing. For example "myLabel"  |

**Example 1**. Run another command `/work` with delay 5 days. And remove that delay (for example on 3th day)

```javascript
Bot.run( {
    command: "/balance",
    run_after: 60*60*24*5,  // 5 days delay
    label: "myLabel"
} )
```

On the third day we learned that the call is no longer needed:

```javascript
// remove all future executions with label "mylabel"
Bot.clearRunAfter( {
    label: "myLabel"
} )
```



## Bot.runAll(options)

Run other command for all chats

{% hint style="success" %}
Use this command for broadcasting any information: message, photo, video, keyboard and etc
{% endhint %}

{% hint style="warning" %}
**Bot.runAll** works for worked bots only. 
{% endhint %}

```javascript
Bot.runAll(params)
```

| Field       | Description                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `command`   | **Required**. Command for run. For example "/start". Can pass params                                                                                                                                          |
| `options`   | json for passing to command. Available through options in this command                                                                                                                                        |
| `for_chats` | <p>Command will be runned for this chats type only. Can be:</p><p><code>"private-chats"</code></p><p><code>"group-chats"</code></p><p><code>"super-group-chats"</code></p><p><code>"all" </code>- default</p> |

**Example**:

Command: `/news`

```javascript
Bot.runAll( {
    // this command will be executed
    // for each private chat (user)
    command: "/broadcast",
    for_chats: "private-chats",
    // options: { any_data: "here" }
} )
```

Command: `/broadcast`

```javascript
// it have user and chat object!
// so we can send any information now: message, keyboard, photo and etc

Bot.sendKeyboard("New news", "hello!")

// we can get brodcast task info
/*
let task = options.task;
Bot.sendMessage(
   "Task progress: " + task.progress +
   "\n total: " + task.total +
   "\n cur index: " + task.cur_position +
   "\n status: " + task.status +
   "\n errors: " + task.errors_count
)
*/
```

