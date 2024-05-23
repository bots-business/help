# Bot functions

| Function                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Bot.runCommand(command, options)`         | <p>Run other command</p><p><code>Bot.runCommand("/contact")</code></p><p><br>and with options: </p><p><code>Bot.runCommand("/contact", {phone: "+15424", email: "example@example.com"})</code></p><p><br>in second command /contact:</p><p>Bot.sendMessage("Phone is:" + options.phone);</p>                                                                                                                                                                    |
| `Bot.run(options)`                         | <p>Run other command</p><p>Bot.run({ command: "/contact" })</p><p></p><p><a href="bot-functions.md#bot.run-params">see more</a></p>                                                                                                                                                                                                                                                                                                                             |
| `Bot.clearRunAfter(options)`               | <p>Clear other command with run_after by label </p><p></p><p><a href="bot-functions.md#bot.clearrunafter-options">see more</a></p>                                                                                                                                                                                                                                                                                                                              |
| `Bot.runAll(options)`                      | <p>Run other command for all chats</p><p><code>Bot.runAll({ command: "/broadcast" })</code></p><p></p><p><a href="bot-functions.md#bot.runall-options">see more</a></p>                                                                                                                                                                                                                                                                                         |
| `Bot.sendKeyboard(buttons, message)`       | <p>send keyboard and message. Message is required</p><p></p><p><code>Bot.sendKeyboard("about, help,\ncontacts", "send keyboard now")</code></p>                                                                                                                                                                                                                                                                                                                 |
| `Bot.sendInlineKeyboard(buttons, message)` | <p>Send inline keyboard and message. Message is required. Buttons is array. Button must have text fields: title(required), url or command.</p><p></p><p><code>Bot.sendInlineKeyboard([ {title: "google", url: "http://google.com" }, {title: "other command", command: "/othercommand"} ], "Please make a choice.")</code></p>                                                                                                                                  |
| `Bot.editInlineKeyboard(buttons)`          | <p>Edit exist inline keyboard after executing the command that was called by its button</p><p></p><p><code>Bot.editInlineKeyboard([ {title: "google", url: "http://google.com" } ])</code></p>                                                                                                                                                                                                                                                                  |
| `Bot.setProperty(name, value, type)`       | <p>Set property with name for bot. <a href="properties.md#set-property">Read more</a></p><p></p><p><code>Bot.setProperty("TotalScore", 100, "integer")</code> </p><p></p><p>Type can be: <code>integer, float, string, text, json, datetime</code></p>                                                                                                                                                                                                          |
| `Bot.getProperty(name)`                    | <p>Read property with name. Name is case sensitive. Name is case sensitive. <a href="properties.md#get-property">Read more.</a></p><p></p><p><code>Bot.getProperty("TotalScore")</code></p><p></p><p>can get property with default value for non exist property:</p><p><code>Bot.getProperty("TotalScore", 100)</code> </p><p><br>can get property of another bot:<br><code>Bot.getProperty({ name: "propName", other_bot_id: OTHER_BOT_ID })</code></p><p></p> |
| `Bot.importCSV()`                          | CSV import. More info [here](https://help.bots.business/create-bot-from-google-table)                                                                                                                                                                                                                                                                                                                                                                           |
| `Bot.blockChat(chat_id)`                   | <p>Block chat:</p><p><code>Bot.blockChat(chat.id)</code></p>                                                                                                                                                                                                                                                                                                                                                                                                    |
| `Bot.unblockChat(chat_id)`                 | <p>Unblock chat:</p><p><code>Bot.unblockChat(chat.id)</code></p>                                                                                                                                                                                                                                                                                                                                                                                                |
| `Bot.inspect(value)`                       | Send inspected value to chat. Good for debug                                                                                                                                                                                                                                                                                                                                                                                                                    |



**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Total score: <TotalScore>!`





## Bot.run(params)

Run other command

```javascript
Bot.run(params)
```

| Field                  | Description                                                                                                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `command`              | **Required**. Command for run. For example "/start". Can pass params                                                                                                                                   |
| `options`              | json for passing to command. Available through options in this command                                                                                                                                 |
| `run_after`            | <p>delay in seconds before command callingName is case sensitive.<br><br>can be float. <br><br>Exact execution time is not guaranteed. Command will be executed in background.</p>                     |
| `background`           | Boolean: (true or false). By default: false. If true - command will be executed in background.                                                                                                         |
| `bot_id`               | <p>bot_id for passing. <strong>By default</strong> this is current bot.id. <br><br>This bot must be in the same BB Account</p>                                                                         |
| `user_id`              | user\_id for passing. **By default** this is current user.id                                                                                                                                           |
| `user_telegramid`      | user\_telegramid for passing                                                                                                                                                                           |
| `chat_id`              | chat\_id for passing. **By default** this is current chat.id                                                                                                                                           |
| `label`                | can be used for clearing with `Bot.clearRunAfter`                                                                                                                                                      |
| `ignoreMissingCommand` | <p>do not throw error if command not found (can be used for some logic with <a href="always-running-commands.md#beforeall-and-afterall-commands">@</a> command and etc).<br><br>By default - false</p> |

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
    // chat_id: chat.id  // or use another chat_id
    user_id: user.id,  // or use another user.id
    // user_telegramid: tgId // or another user's telegram id
    // bot_id: ANOTHER_BOT_ID // to run command for your another bot 
} )
```

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
Bot.run({
    command: "/balance",
    run_after: 60*60*24*5,  // 5 days delay
    label: "myLabel"
})
```

On the third day we learned that the call is no longer needed:

```javascript
// remove all future executions with label "mylabel"
Bot.clearRunAfter({
    label: "myLabel"
})
```



## Bot.runAll(options)

Run other command for all chats

{% hint style="success" %}
Use this command for broadcasting any information: message, photo, video, keyboard and etc
{% endhint %}

{% hint style="warning" %}
**Bot.runAll** works for worked bots only. \
\
If you start a new task **before** the previous old task has completed, there is **no guarantee** that the old task will complete for all chats.



You **need to wait** for the old task to complete before starting the new one.
{% endhint %}





<pre class="language-javascript"><code class="lang-javascript"><strong>Bot.runAll(params)
</strong></code></pre>

| Field       | Description                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `command`   | **Required**. Command for run. For example "/start". Can pass `params`                                                                                                                                        |
| `options`   | json for passing to command. Available through options in this command                                                                                                                                        |
| `on_create` | run this command on task creation with task information                                                                                                                                                       |
| `for_chats` | <p>Command will be runned for this chats type only. Can be:</p><p><code>"private-chats"</code></p><p><code>"group-chats"</code></p><p><code>"super-group-chats"</code></p><p><code>"all"</code> - default</p> |

**Example**:

Command: `/news`

```javascript
Bot.runAll( {
    // this command will be executed
    // for each private chat (user)
    command: "/broadcast",
    for_chats: "private-chats",
    on_create: "on_new_brodcast_task",
    // you can pass data via options
    options: { news: "Hello! it is news!" }
} )
```

Command: `/broadcast`

```javascript
// it have user and chat object!
// so we can send any information now: message, keyboard, photo and etc

Bot.sendMessage(options.news)

// we can get brodcast info via task
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

Command: `on_new_brodcast_task`

```javascript
const task = options.run_all_task;
Bot.sendMessage(
  "Task for brodcasting created. Task id: " + task.id
);

// save task id:
Bot.setProperty("curBrodcastTaskID", task.id, "integer")

// Bot.inspect(options.run_all_task);
```

Command: `/progress`

```javascript
// show current runAll progress

const taskID = Bot.getProperty("curBrodcastTaskID");
let task = new RunAllTask({ id: taskID });
// Bot.inspect(task) // you can check all fields

if(!task.status){
  Bot.sendMessage("This task not found")
  return
}

Bot.sendMessage(
  "Current brodcast: " + 
  "\n Status: " + task.status + " " + task.progress + "%" +
  "\n Progress:" + task.cur_position + "/" + task.total
)

```
