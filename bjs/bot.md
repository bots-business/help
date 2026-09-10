---
description: Reference for BJS Bot methods including sending and editing messages, keyboards, command calls, scheduling, properties,
  caching, chat blocking, and Git operations.
---


# Bot methods

Use `Bot` for Bots.Business actions such as sending a reply, running a command, saving a property, or scheduling work. Most action methods queue work; they do not return a Telegram response. Use [Api callbacks](telegram-api.md) when you need a sent message's ID.

## Send and inspect messages

{% code title="/hello" overflow="wrap" %}
```javascript
// Command: /hello
Bot.sendMessage("Hello!");
Bot.sendMessage("Literal *characters*", { parse_mode: null });
```
{% endcode %}

| Method | Parameters and behavior |
| --- | --- |
| `Bot.sendMessage(text, options)` | Text for the current chat; optional message options such as `parse_mode` |
| `Bot.sendMessage({text, ...})` | Object form; `text` is required; additional supported message options go in the same object |
| `Bot.inspect(value)` | Formats a value as JSON and sends it as plain text; useful for test data |
| `Bot.sendMessageToChat(name, text, options)` | Sends to a chat already known to this bot, matched by its title |
| `Bot.sendMessageToChatWithId(id, text, options)` | Sends to a known chat by its **Telegram chat ID**, such as `chat.chatid` |

Prefer an explicit chat ID over a title when titles could be duplicated or changed. These helpers do not discover arbitrary chats. Never inspect an entire token-bearing object or a response containing credentials in a public chat.

For precise Telegram options such as `message_thread_id`, media, reply parameters, or callbacks, use [Api](telegram-api.md).

## Edit messages

| Method | Parameters |
| --- | --- |
| `Bot.editMessage(text, message_id, options)` | Edit a bot message in the current chat |
| `Bot.editMessageInChat(chat_id, text, message_id)` | Edit a bot message in another known chat; `chat_id` is the Telegram chat ID |
| `Bot.editInlineKeyboard(buttons, message_id, chat_id)` | Replace an inline keyboard; use Telegram message/chat IDs |

{% hint style="info" %}
Store the Telegram `message_id` together with its chat ID. A message ID is unique **within its chat**, not across every chat of your bot. Telegram also limits which messages a bot can edit. [Telegram Message reference](https://core.telegram.org/bots/api#message).
{% endhint %}

For a complete send-result-edit example, see [Telegram API](telegram-api.md#save-a-message-id-and-edit-the-message).

## Keyboards

Command `/menu`:

{% code title="/menu" overflow="wrap" %}
```javascript
Bot.sendInlineKeyboard([
  { title: "Help", command: "/help" },
  { title: "Website", url: "https://bots.business/" }
], "Choose an option:");
```
{% endcode %}

Create `/help` separately:

{% code title="Keyboards · Example 3" overflow="wrap" %}
```javascript
Bot.sendMessage("Send /menu to see your options.");
```
{% endcode %}

| Method | Use |
| --- | --- |
| `Bot.sendKeyboard(buttons, text, options)` | Reply keyboard; button labels become messages. See the [reply keyboard guide](../app/reply-keyboard.md). |
| `Bot.sendInlineKeyboard(buttons, text, options)` | Inline buttons attached to the message; buttons use `title` and `command` or `url` |
| `Bot.sendInlineKeyboardToChatWithId(chat_id, buttons, text, options)` | Inline keyboard sent to a Telegram chat ID |

Nested arrays group inline buttons into rows. `Api.sendMessage` uses Telegram's different keyboard shape (`text`, `callback_data`, `inline_keyboard`); do not mix the two formats. See [inline interactions](inline.md).

### Remove a reply keyboard

Send a message with Telegram's keyboard-removal markup from an incoming-chat command:

{% code title="Remove a reply keyboard · Example 4" overflow="wrap" %}
```javascript
if (!chat) { return; }
Api.sendMessage({
  chat_id: chat.chatid,
  text: "You can type your next message.",
  reply_markup: { remove_keyboard: true }
});
```
{% endcode %}

This removes a reply keyboard. To change inline buttons attached to a specific message, use the inline editing methods instead.

## Run and schedule commands

| Method | Behavior |
| --- | --- |
| `Bot.runCommand(command, options)` | Run a named command with optional structured options |
| `Bot.run({command, options, ...})` | Command call with explicit scheduling/context options |
| `Bot.clearRunAfter({label})` | Delete this bot's pending scheduled requests with the label |
| `Bot.runAll({command, ...})` | Create a task that runs a command for multiple chats |

`Bot.run` supports `run_after` in seconds, `label`, internal `bot_id`, `user_id`, `chat_id`, Telegram `user_telegramid`, and `ignoreMissingCommand`. A custom destination may cause the runtime to schedule a one-second delay even when no delay was supplied. The `background` field is not forwarded by this method; use `run_after` for delayed execution.

{% hint style="warning" %}
`Bot.clearRunAfter()` without a label clears pending requests for the whole bot. Use a specific label for a user's reminder. See [scheduling and hooks](background.md) and [broadcasts](broadcasts.md).
{% endhint %}

## Data and cached commands

| Methods | Reference |
| --- | --- |
| `setProp`, `getProp`, `deleteProp` | [User and bot properties](user-properties.md); `setProperty` and `getProperty` remain compatible names |
| `setCache(seconds)`, `clearCache(command_name)` | [Caching](caching.md) |

## Chat access and imports

`Bot.blockChat(chat.id)` marks an internal chat as blocked by the administrator. `Bot.unblockChat(chat.id)` removes that administrator block; it cannot make a Telegram user unblock the bot. These methods use **internal chat IDs**, unlike the send-to-chat helpers above.

`Bot.importCSV()` starts an import using the bot's configured [CSV source](../integrations/google-table-import.md). `Bot.importGit({branch, success})` and `Bot.exportGit({branch, success})` use its [Git configuration](../integrations/git.md); `success` names a callback command. Export is refused for protected bots. Imports can change bot commands, so run them deliberately after configuring and checking the source in the app.

## Troubleshooting

If nothing happens, check the destination's ID type, the callback command's name, bot permissions, and the error log. If several command calls stop partway through, reduce the chain: nested command execution is limited. A misspelled or obsolete method is not an alias; prefer the methods listed here and [diagnose BJS errors](errors.md).
