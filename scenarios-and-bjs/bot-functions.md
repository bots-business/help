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
        <p><b>DEPRECATED</b> Run other command</p>
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
      <td style="text-align:left">Bot.run(options)</td>
      <td style="text-align:left">
        <p>Run other command</p>
        <p>Bot.run({ command: &quot;/contact&quot; })</p>
        <p></p>
        <p>see more</p>
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
        <p>Read property with name</p>
        <p></p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.importCSV()</code>
      </td>
      <td style="text-align:left">CSV import. More info <a href="https://help.bots.business/create-bot-from-google-table">here</a>
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
| `run_after` | delay in seconds before command calling |
| `user_id` | user\_id for passing. By default this is current user.id |
| `chat_id` | chat\_id for passing. By default this is current chat.id |

**Example 1**. Run another command `/balance` with delay 1 hour for another user

```javascript
Bot.run( {
    command: "/balance",
    run_after: 1*60*60,  // 1 hour delay
} )
```

**Example 2**. Run another command `/balance` with delay 5 days for another user

```javascript
Bot.run( {
    command: "/balance",
    run_after: 60*60*24*5,  // 5 days delay
    // options: { amount: 5, currency: "BTC" }  // you can pass data
    chat_id: ANOTHER_CHAT_ID
    user_id: ANOTHER_USER_ID
} )
```



