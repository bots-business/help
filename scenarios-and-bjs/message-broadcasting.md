# Message broadcasting and editing



<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessage(text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to current chat</p>
        <p></p>
        <p><code>Bot.sendMessage(&quot;Hello from bot&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChatWithId(chatid, text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to chat with id. Current chatid for chat is contained in
          chat.chatid</p>
        <p></p>
        <p><code>Bot.sendMessageToChatWithId(&quot;45445454521&quot;, &quot;Hello users!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChat(chat_name, text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to other chat. The bot must be installed in another chat
          room</p>
        <p></p>
        <p><code>sendMessageToChat(&quot;OtherTestChat&quot;, &quot;Hello to all!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllPrivateChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to all private chats</p>
        <p></p>
        <p><code>Bot.sendMessageToAllPrivateChats(&quot;Hello user&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to all group chats</p>
        <p></p>
        <p><code>Bot.sendMessageToAllGroupChats(&quot;this is the group chat&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllSuperGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to all super group chat</p>
        <p></p>
        <p><code>Bot.sendMessageToAllSuperGroupChats(&quot;You in super group!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Send message to all chat</p>
        <p></p>
        <p><code>Bot.sendMessageToAllChats(&quot;This message for all users&quot;)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>**Security**

{% hint style="danger" %}
Please note that the user can create a chat with any name and add to it your bot.

Therefore, the function `Bot.sendMessageToChatWithId` is more preferable than the function `Bot.sendMessageToChat`.
{% endhint %}

## **Message editing**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Function</b>
      </th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">editMessage(value, message_id, options)</td>
      <td style="text-align:left">
        <p>edit message with value and message_id</p>
        <p></p>
        <p><code>Bot.editMessage(&quot;new text&quot;, 20)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">editMessageInChat(chat_id, value, message_id)</td>
      <td style="text-align:left">
        <p>edit message with value and message_id in chat</p>
        <p></p>
        <p><code>Bot.editMessageInChat(10512154, &quot;new text&quot;, 25)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
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

| Parameter | Type | Description |
| :--- | :--- | :--- |
| disable\_web\_page\_preview | Boolean | Disables link previews for links in this message |
| disable\_notification | Boolean | Sends the message silently. Users will receive a notification with no sound. |
| reply\_to\_message\_id | Integer | If the message is a reply, ID of the original message |
| is\_reply | Boolean | If the message is a reply for previous message |
| parse\_mode | String | Send `Markdown` or `HTML`, if you want Telegram apps to show bold, italic, fixed-width text or inline URLs in your bot's message. Default `Markdown`. Can be `Markdown`, `HTML` or `null` |
| result\_to\_bot\_property | String | It is preferable to use "on\_result". Store result of message sending in bot property with this name.  You can read this result later in other commands by Bot.getProperty |
| on\_result | String | Call this command after method with result  |

\`\`

