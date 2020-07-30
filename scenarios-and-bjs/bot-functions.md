# Bot functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.runCommand(command, options)</code>
      </td>
      <td style="text-align:left">
        <p>Run other command</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;)</code>
        </p>
        <p>
          <br />and with options:</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;, {phone: &quot;+15424&quot;, email: &quot;example@example.com&quot;})</code>
        </p>
        <p>
          <br />in second command /contact:</p>
        <p>Bot.sendMessage(&quot;Phone is:&quot; + options.phone);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.run(options)</code>
      </td>
      <td style="text-align:left">
        <p>Run other command</p>
        <p>Bot.run({ command: &quot;/contact&quot; })</p>
        <p></p>
        <p><a href="https://help.bots.business/scenarios-and-bjs/bot-functions#bot-run-options">see more</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.clearRunAfter(options)</code>
      </td>
      <td style="text-align:left">
        <p>Clear other command with run_after by label</p>
        <p></p>
        <p><a href="https://help.bots.business/scenarios-and-bjs/bot-functions#bot-clearrunafter-options">see more</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.runAll(options)</code>
      </td>
      <td style="text-align:left">
        <p>Run other command for all chats</p>
        <p><code>Bot.runAll({ command: &quot;/broadcast&quot; })</code>
        </p>
        <p></p>
        <p><a href="https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options">see more</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>send keyboard and message. Message is required</p>
        <p></p>
        <p><code>Bot.sendKeyboard(&quot;about, help,\ncontacts&quot;, &quot;send keyboard now&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>Send inline keyboard and message. Message is required. Buttons is array.
          Button must have text fields: title(required), url or command.</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;other command&quot;, command: &quot;/othercommand&quot;} ], &quot;Please make a choice.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboardToChatWithId(chat_id, buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>Send inline keyboard and message to chat with chat_id</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard(&apos;852378745487&apos;, [ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;other command&quot;, command: &quot;/othercommand&quot;} ], &quot;Please make a choice.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons)</code>
      </td>
      <td style="text-align:left">
        <p>Edit exist inline keyboard after executing the command that was called
          by its button</p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ])</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons, message_id, chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Edit exist inline keyboard for message with message_id in the chat with
          chat_id. If chat_id is blank current chat is used</p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ], request.message.message_id, chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>Set property with name for bot</p>
        <p></p>
        <p><code>Bot.setProperty(&quot;TotalScore&quot;, 100, &quot;integer&quot;)</code> 
        </p>
        <p></p>
        <p>Type can be integer, float, string, text, json, datetime</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.getProperty(name)</code>
      </td>
      <td style="text-align:left">
        <p>Read property with name. Name is case sensitive. Name is case sensitive.</p>
        <p></p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;)</code>
        </p>
        <p></p>
        <p>can get property with default value for non exist property:</p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;, 100)</code> 
        </p>
        <p>
          <br />can get property of another bot:
          <br /><code>Bot.getProperty({ name: &quot;propName&quot;, other_bot_id: OTHER_BOT_ID })</code>
        </p>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.importCSV()</code>
      </td>
      <td style="text-align:left">CSV import. More info <a href="https://help.bots.business/create-bot-from-google-table">here</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.blockChat(chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Block chat:</p>
        <p><code>Bot.blockChat(chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.unblockChat(chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Unblock chat:</p>
        <p><code>Bot.unblockChat(chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.inspect(value)</code>
      </td>
      <td style="text-align:left">Send inspected value to chat. Good for debug</td>
    </tr>
  </tbody>
</table>



**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Total score: <TotalScore>!`

in BJS:

> And you can use it in `Bot.sendMessage("<TotalScore>")`



## Bot.run\(options\)

Run other command

```javascript
Bot.run(params)
```

| Field | Description |
| :--- | :--- |
| `command` | **Required**. Command for run. For example "/start". Can pass params  |
| `options` | json for passing to command. Available through options in this command |
| `run_after` | delay in seconds before command callingName is case sensitive. |
| `bot_id` | bot\_id for passing. **By default** this is current bot.id |
| `user_id` | user\_id for passing. **By default** this is current user.id |
| `chat_id` | chat\_id for passing. **By default** this is current chat.id |
| `label` | can be used for clearing with `Bot.clearRunAfter` |

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

## Bot.clearRunAfter\(options\)

Can clear future command\(s\) execution setted by Bot.run

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

| Field | Description |
| :--- | :--- |
| `label` | **Required**. Command for clearing. For example "myLabel"  |

**Example 1**. Run another command `/work` with delay 5 days. And remove that delay \(for example on 3th day\)

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



## Bot.runAll\(options\)

Run other command for all chats

{% hint style="success" %}
Use this command for broadcasting any information: message, photo, video, keyboard and etc
{% endhint %}

```javascript
Bot.runAll(params)
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>command</code>
      </td>
      <td style="text-align:left"><b>Required</b>. Command for run. For example &quot;/start&quot;. Can
        pass params</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>options</code>
      </td>
      <td style="text-align:left">json for passing to command. Available through options in this command</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>for_chats</code>
      </td>
      <td style="text-align:left">
        <p>Command will be runned for this chats type only. Can be:</p>
        <p><code>&quot;private-chats&quot;</code>
        </p>
        <p><code>&quot;group-chats&quot;</code>
        </p>
        <p><code>&quot;super-group-chats&quot;</code>
        </p>
        <p><code>&quot;all&quot; </code>- default</p>
      </td>
    </tr>
  </tbody>
</table>

**Example**:

Command: `/news`

```javascript
Bot.runAll( {
    // this command will be executed
    // for each private chat (user)
    command: "/broadcast",
    for_chats: "private-chats"
} )
```

Command: `/broadcast`

```javascript
// it have user and chat object!
// so we can send any information now: message, keyboard, photo and etc

Bot.sendKeyboard("New news", "hello!")
```

