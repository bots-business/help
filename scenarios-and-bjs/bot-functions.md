# Функции бота

<table>
  <thead>
    <tr>
      <th style="text-align:left">Функции</th>
      <th style="text-align:left">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.runCommand(command, options)</code>
      </td>
      <td style="text-align:left">
        <p>Запустить другую команду</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;)</code>
        </p>
        <p>
          <br />и с опцией:</p>
        <p><code>Bot.runCommand(&quot;/contact&quot;, {phone: &quot;+15424&quot;, email: &quot;example@example.com&quot;})</code>
        </p>
        <p>
          <br />во второй команде /contact:</p>
        <p>Bot.sendMessage(&quot;Phone is:&quot; + options.phone);</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить клавиатуру и сообщение. Сообщение необходимо</p>
        <p></p>
        <p><code>Bot.sendKeyboard(&quot;about, help,\ncontacts&quot;, &quot;send keyboard now&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboard(buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить inline клавиатуру и сообщение. Сообщение обязательно. Кнопки это массив.
          Кнопки должны иметь поле с текстом: title("заголовок" обязателен), url или command.</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;другая команда&quot;, command: &quot;/othercommand&quot;} ], &quot;Пожалуйста выберите.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendInlineKeyboardToChatWithId(chat_id, buttons, message)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить inline клавиатуру и сообщение в чат с chat_id</p>
        <p></p>
        <p><code>Bot.sendInlineKeyboard(&apos;852378745487&apos;, [ {title: &quot;google&quot;, url: &quot;http://google.com&quot; }, {title: &quot;другая команда&quot;, command: &quot;/othercommand&quot;} ], &quot;Пожалуйста выберите.&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons)</code>
      </td>
      <td style="text-align:left">
        <p>Редактировать выведенную inline клавиатуру после запуска вызванной команды 
          при помощи ее кнопки</p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ])</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.editInlineKeyboard(buttons, message_id, chat_id)</code>
      </td>
      <td style="text-align:left">
        <p>Редактировать сообщение выведенной inline клавиатуры с message_id в чате
          с помощью chat_id. Если chat_id пустой то используется текущий чат </p>
        <p></p>
        <p><code>Bot.editInlineKeyboard([ {title: &quot;google&quot;, url: &quot;http://google.com&quot; } ], request.message.message_id, chat.id)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.setProperty(name, value, type)</code>
      </td>
      <td style="text-align:left">
        <p>Установить свойство с именем для бота</p>
        <p></p>
        <p><code>Bot.setProperty(&quot;TotalScore&quot;, 100, &quot;integer&quot;)</code> 
        </p>
        <p></p>
        <p>Типом может быть целое число, число с плавающей точкой, строка, текст, JSON, дата и время</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.getProperty(name)</code>
      </td>
      <td style="text-align:left">
        <p>Прочитать свойство с именем</p>
        <p></p>
        <p><code>Bot.getProperty(&quot;TotalScore&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.importCSV()</code>
      </td>
      <td style="text-align:left">Импорт CSV. Больше информации <a href="https://help.bots.business/create-bot-from-google-table">здесь</a>
      </td>
    </tr>
  </tbody>
</table>

**Доступ к свойствам** в ответах(answer):

> Вы также можете использовать свойства в ответе команды. Например, вы можете сделать это с помощью команды / hello: `Общие очки: <TotalScore>!`

в BJS:

> И вы можете использовать его в 
    `Bot.sendMessage ("<TotalScore>")`

