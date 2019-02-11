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
        <p>Bot.sendMessage("Phone is:" + options.phone);</p>
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
  </tbody>
</table>

**Access to property** in answer:

> You can also use the properties in the command's answer. For example, you can do this with the / hello command:`Total score: <TotalScore>!`

in BJS:

> And you can use it in `Bot.sendMessage("<TotalScore>")`

