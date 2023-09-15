# Message broadcasting and editing

| Function                  | Description                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Bot.sendMessage(text)`   | <p>Send message to current chat. It is simple method with markdown by default.</p><p></p><p><code>Bot.sendMessage("Hello from bot")</code></p>                                                                                                                                                                                                                                                                           |
| `Api.sendMessage(params)` | <p>Send message. It is Telegram Bot Api <a href="https://core.telegram.org/bots/api#sendmessage">method</a>. You can pass any params like text, reply_markup, parse_mode and etc:<br><br><code>Api.sendMessage({</code><br>   <code>text: "Hello, &#x3C;b>World!&#x3C;/b>",</code></p><p>   <code>parse_mode: "HTML",</code><br><code>})</code><br><br>By default, chat_id accord to the current chat. (chat.chatid)</p> |



## Do you want broadcast text to all chats?

See [Bot.runAll](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command

## Do you want broadcast photo, video and etc?

See [Bot.runAll](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command





## **Message editing**

| **Function**                        | Description                                                                                                                  |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Bot.editMessage(value, message\_id) | <p>Simple method for message editing with value and message_id</p><p></p><p><code>Bot.editMessage("new text", 20)</code></p> |
| Api.editMessageText(params)         | Advanced method for message editing. Please see full description [here](https://core.telegram.org/bots/api#editmessagetext). |

{% hint style="info" %}
message\_id - it is unique identificator for all chats of this bot.
{% endhint %}

{% hint style="info" %}
We have several methods for editing:&#x20;

`Api.editMessageText`

`Api.editMessageCaption`

`Api.editMessageMedia`

`Api.editMessageLiveLocation` and etc. Please see [here](https://core.telegram.org/bots/api#editmessagetext).
{% endhint %}

### **Message\_id for income messages to bot**

For income messages to bot: use `request.message_id`

#### Example

```javascript
let msg_id = request.message_id;
Bot.editMessage("new text", msg_id);
```

{% hint style="warning" %}
Message\_id - have unique value for all chats of bot. So we have only one message\_id with value "2" and only in one chat.
{% endhint %}

###

### Bot message removing

In this example bot will remove old messages from bot.

in first command:

```javascript
Api.sendMessage({
  text: "Hello!",
  // we going to remove this message after 120 sec
  on_result: "removeMsgAfter 120"
})
```

in command `removeMsgAfter`:

```javascript
// user can run this command manually
if(!options){ return }
if(!options.result.message_id){ return }

// extract time delay
let runAfter = parseInt(params);

// run message removing after "runAfter" minutes
Bot.run({
  command: "removeMsg",
  options: { message_id: options.result.message_id },
  run_after: runAfter // in seconds
})
```

in command `removeMsg:`

```javascript
if(!options){ return }

// remove message
Api.deleteMessage({
  message_id: options.message_id
})

// also you can edit message here, make message forwarding and etc
// you have message_id here - so you can do anything
```

