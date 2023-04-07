# Message broadcasting and editing



| Function                                    | Description                                                                                                                                                                   |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Bot.sendMessage(text)`                     | <p>Send message to current chat</p><p></p><p><code>Bot.sendMessage("Hello from bot")</code></p>                                                                               |
| `Bot.sendMessageToChatWithId(chatid, text)` | <p>Send message to chat with id. Current chatid for chat is contained in chat.chatid</p><p></p><p><code>Bot.sendMessageToChatWithId("45445454521", "Hello users!")</code></p> |
| `Bot.sendMessageToChat(chat_name, text)`    | <p>Send message to other chat. The bot must be installed in another chat room</p><p></p><p><code>sendMessageToChat("OtherTestChat", "Hello to all!")</code></p>               |

## Do you want broadcast text?

See [Bot.runAll](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command

## Do you want broadcast photo, video and etc?

See [Bot.runAll](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command



**Security**

{% hint style="danger" %}
Please note that the user can create a chat with any name and add to it your bot.

Therefore, the function `Bot.sendMessageToChatWithId` is more preferable than the function `Bot.sendMessageToChat`.
{% endhint %}

## **Message editing**

| **Function**                                    | Description                                                                                                                     |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| editMessage(value, message\_id, options)        | <p>edit message with value and message_id</p><p></p><p><code>Bot.editMessage("new text", 20)</code></p>                         |
| editMessageInChat(chat\_id, value, message\_id) | <p>edit message with value and message_id in chat</p><p></p><p><code>Bot.editMessageInChat(10512154, "new text", 25)</code></p> |

{% hint style="info" %}
message\_id - it is unique identificator for all chats of this bot.
{% endhint %}

### **Message\_id for income messages to bot**

For income messages to bot: use `request.message_id`

#### Example

```javascript
let msg_id = request.message_id;
Bot.editMessage("new text", msg_id)

// also you can store message_id for future editing:
User.setProperty("msg_id", msg_id, "integer");
// or if you want to edit message from others chats:
Bot.setProperty("msg_id" + chat.chatid, msg_id, "integer");
```

{% hint style="warning" %}
Message\_id - have unique value for all chats of bot. So we have only one message\_id with value "2" and only in one chat.
{% endhint %}

### **Message\_id for** messages from bot

use `on_result` in options - see [this](https://help.bots.business/scenarios-and-bjs/message-broadcasting#options-for-sendmessage-editmessage-and-sendkeyboard-functionals-reply-disable-notification-disable-web-page-preview)

#### Example

in first command:

```javascript
Bot.sendMessage("hello",
   {on_result: "onMessageSending" }
)
```

in command `onMessageSending`:

```javascript
// You can inspect all result:
// Bot.inspect(options)

let msg_id = options.result.message_id;
Bot.editMessage("new text", msg_id);

// Also you can save message_id for future:
// Bot.setProperty( "MSG-for-edit" + chat.chatid, msg_id, "integer");
```



### **Options for sendMessage, editMessage and sendKeyboard functionals: Reply, Disable Notification, Disable web page preview**

You can pass options parameter to any `sendMessageXXX`, `sendKeyboard`, `editMesage`, `editMessageInChat` and`sendInlineKeyboard` functions:

```javascript
  let options = { disable_notification: true, reply_to_message_id: request.message_id };
  Bot.sendMessage("Hello from bot", options);
  Bot.sendMessageToChatWithId("45445454521", "Hello users!", options);
  Bot.sendKeyboard("about, help,\ncontacts", "send keyboard now", options)
```

| Parameter                   | Type    | Description                                                                                                                                                                               |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| disable\_web\_page\_preview | Boolean | Disables link previews for links in this message                                                                                                                                          |
| disable\_notification       | Boolean | Sends the message silently. Users will receive a notification with no sound.                                                                                                              |
| reply\_to\_message\_id      | Integer | If the message is a reply, ID of the original message                                                                                                                                     |
| is\_reply                   | Boolean | If the message is a reply for previous message                                                                                                                                            |
| parse\_mode                 | String  | Send `Markdown` or `HTML`, if you want Telegram apps to show bold, italic, fixed-width text or inline URLs in your bot's message. Default `Markdown`. Can be `Markdown`, `HTML` or `null` |
| result\_to\_bot\_property   | String  | It is preferable to use "on\_result". Store result of message sending in bot property with this name.  You can read this result later in other commands by Bot.getProperty                |
| on\_result                  | String  | Call this command after method with result                                                                                                                                                |

