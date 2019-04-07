# Обработка и редактирование сообщений



<table>
  <thead>
    <tr>
      <th style="text-align:left">Функция</th>
      <th style="text-align:left">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessage(text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение в текущий чат</p>
        <p></p>
        <p><code>Bot.sendMessage(&quot;Привет от бота&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChatWithId(chatid, text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение в чат с id. Текущий chatid для чата содержится в          data.chat.chatid</p>
        <p></p>
        <p><code>Bot.sendMessageToChatWithId(&quot;45445454521&quot;, &quot;Привет юзер!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToChat(chat_name, text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение в другой чат. Бот должен быть установлен в том, пользовательском,чате 
          </p>
        <p></p>
        <p><code>sendMessageToChat(&quot;OtherTestChat&quot;, &quot;Привет всем!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllPrivateChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение во все личные чаты</p>
        <p></p>
        <p><code>Bot.sendMessageToAllPrivateChats(&quot;Привет пользователь&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение во все групповые чаты</p>
        <p></p>
        <p><code>Bot.sendMessageToAllGroupChats(&quot;Это групповой чат &quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllSuperGroupChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение во все супер группы chat</p>
        <p></p>
        <p><code>Bot.sendMessageToAllSuperGroupChats(&quot;Вы находитесь в супер группе!&quot;)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Bot.sendMessageToAllChats(text)</code>
      </td>
      <td style="text-align:left">
        <p>Отправить сообщение во все чаты</p>
        <p></p>
        <p><code>Bot.sendMessageToAllChats(&quot;Это сообщение для всех пользователей&quot;)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>**Безопасность**

{% hint style="danger" %}
Обратите внимание, что пользователь может создать чат с любым именем и добавить в него своего бота.

Поэтому функция `Bot.sendMessageToChatWithId` более предпочтительна, чем функция
 `Bot.sendMessageToChat`.
{% endhint %}

## **Редактирование сообщений**

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Функция</b>
      </th>
      <th style="text-align:left">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">editMessage(value, message_id, options)</td>
      <td style="text-align:left">
        <p>Редактирует сообщение с помощью переменной и message_id(id сообщения)</p>
        <p></p>
        <p><code>Bot.editMessage(&quot;Новый текст&quot;, 20)</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">editMessageInChat(chat_id, value, message_id)</td>
      <td style="text-align:left">
        <p>Редактирует сообщение с переменной и message_id в чате</p>
        <p></p>
        <p><code>Bot.editMessageInChat(10512154, &quot;Новый текст&quot;, 25)</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
message\_id - это уникальный идентификатор для всех чатов этого бота.
{% endhint %}

### **Message\_id входящего сообщения**

Для входящего сообщения в бот: используйте `request.message_id`

#### Например

```javascript
let msg_id = request.message_id;
Bot.editMessage("Новый текст", msg_id)

// также вы можете сохранить message_id для дальнейшего изменения:
User.setProperty("msg_id", msg_id, "integer");
// или если вы хотите редактировать сообщение других чатов:
Bot.setProperty("msg_id" + chat.chatid, msg_id, "integer");
```

{% hint style="warning" %}
Message\_id - имеет уникальную переменную для всех чатов бота. Значит мы можем иметь только один message\_id со значение "2" и только в одном чате.
{% endhint %}

### **Message\_id для** сообщений бота

используйте `result_to_bot_property` в опциях - См. [здесь](https://help.bots.business/scenarios-and-bjs/message-broadcasting#options-for-sendmessage-editmessage-and-sendkeyboard-functionals-reply-disable-notification-disable-web-page-preview)

#### Например

в первой команде:

```javascript
Bot.sendMessage("Привет",
   {result_to_bot_property: "MSG-for-edit" + chat.chatid }
)
```

в другой команде:

```javascript
let msg_id = Bot.getProperty( "MSG-for-edit" + chat.chatid);
Bot.editMessage("новый текст", msg_id)
```



### **Опции(options) для функций sendMessage, editMessage и sendKeyboard: Ответить, Отключить уведомление, Отключить предварительный просмотр веб-страницы**

 Вы можете передать параметр options любому из функций`sendMessageXXX`, `sendKeyboard`, `editMesage`, `editMessageInChat` и`sendInlineKeyboard`:

```javascript
  let options = { disable_notification: true, reply_to_message_id: request.message_id };
  Bot.sendMessage("Привет от бота", options);
  Bot.sendMessageToChatWithId("45445454521", "Привет пользователь!", options);
  Bot.sendKeyboard("О нас, Помощь,\nКонтакты", "Отправьте клавиатуру", options)
```

| Parameter(параметр) | Тип | Описание |
| :--- | :--- | :--- |
| disable\_web\_page\_preview | Логический | Отключает предварительный просмотр ссылок в этом сообщении |
| disable\_notification | Логический | Отправляет сообщение молча. Пользователи получат уведомление без звука. |
| reply\_to\_message\_id | Целое число | Если сообщение является ответом, ID исходного сообщения |
| is\_reply | Логический | Если сообщение является ответом на предыдущее сообщение |
| parse\_mode | Строка | Отправляет `Markdown` или `HTML`, если вы хотите чтобы Telegram печатал жирным, кривым,текстом фиксированной ширины или inline ссылка в сообщении вашего бота. По умолчанию `Markdown`. Возможны `Markdown`, `HTML` или `null(пустое значение)` |
| result\_to\_bot\_property | Строка | Сохранить результат отправки сообщения в свойстве бота с таким именем. Вы можете прочитать этот результат позже в других командах с помощью Bot.getProperty |

## `Новый функционал в разработке..`

Данная функция в прогрессе

`Bot.sendMessage(options)`

опциями могут быть `{text: 'MESSAGE', result_to_bot_property: 'PropertyName'}`

`result_to_bot_property` - может прочитать результат отправляемого сообщения \(со статусом, message\_id\)

