---
description: Call Telegram Bot API methods from BJS, handle on_result and on_error callbacks, retain message IDs, and build
  reply markup correctly.
---


# Telegram API with Api

Use `Api` for Telegram methods such as `sendMessage`, `sendPhoto`, `editMessageText`, and `getChatMember`. Supply a parameter object. When the current execution has a chat, Bots.Business fills a missing `chat_id` from that chat's Telegram ID.

## Send a message

{% code title="/hello" overflow="wrap" %}
```javascript
// Command: /hello
Api.sendMessage({
  text: "Hello from <b>Bots.Business</b>!",
  parse_mode: "HTML"
});
```
{% endcode %}

Telegram controls the parameters, permissions, and restrictions of individual methods. The [Telegram Bot API reference](https://core.telegram.org/bots/api) is the parameter reference; the method must also be supported by Bots.Business.

## Reply to the user's message

Create `/reply` with empty **Answer** and **Keyboard**, **Wait for answer** off and no Auto Retry interval:

{% code title="/reply" overflow="wrap" %}
```javascript
// Command: /reply
if (!chat || !request || !request.message_id) { return; }
Api.sendMessage({
  chat_id: chat.chatid,
  text: "This replies to your message.",
  reply_parameters: { message_id: request.message_id }
});
```
{% endcode %}

Send `/reply` in a private Telegram chat. The bot's answer should show your command as the message being replied to. `request.message_id` identifies the incoming message; `chat.chatid` is its Telegram chat ID. `reply_parameters` selects that message in the same chat. A callback query has a different request shape, so this example is for an incoming message. [Telegram ReplyParameters](https://core.telegram.org/bots/api#replyparameters).

## Receive success and failure callbacks

`Api` calls do not return the remote result to the next JavaScript line. Add callback **command names** to the parameter object:

| Bots.Business option | Behavior |
| --- | --- |
| `on_result` | Command called with the successful API response in `options` |
| `on_error` | Command called with error details in `options.error` |
| `bb_options` | Extra structured context attached to the callback response |
| `result_to_bot_property` | Save the successful response as a JSON bot property |
| `result_to_user_property` | Save the successful response as a JSON current-user property |

These fields are removed before forwarding the request to Telegram. Use one result-property destination at a time. Result-property saving happens after the result callback, so read the response from `options` inside that callback.

Command `/send-status`:

{% code title="/send-status" overflow="wrap" %}
```javascript
Api.sendMessage({
  text: "Your request is ready.",
  on_result: "/sent-status",
  on_error: "/send-failed",
  bb_options: { source: "status" }
});
```
{% endcode %}

Command `/sent-status`:

{% code title="/sent-status" overflow="wrap" %}
```javascript
if (!options || !options.result) { return; }
User.setProp("last_status_message", {
  message_id: options.result.message_id,
  chat_id: options.result.chat.id
}, "json");
```
{% endcode %}

Command `/send-failed`:

{% code title="/send-failed" overflow="wrap" %}
```javascript
if (!options || !options.error) { return; }
// Save only a bounded diagnostic for your own bot.
Bot.setProp("last_send_error", String(options.error).slice(0, 500));
```
{% endcode %}

Avoid responding to every send failure with another send attempt: the destination may have blocked the bot. An `on_error` callback takes over handling of that failure; account for unavailable destinations in your own flow.

## Save a message ID and edit the message

Run `/send-status` above first. Then create `/update-status`:

{% code title="Save a message ID and edit the message · Example 6" overflow="wrap" %}
```javascript
if (!user) { return; }
var sent = User.getProp("last_status_message");
if (!sent) {
  Bot.sendMessage("Use /send-status first.");
  return;
}
Api.editMessageText({
  chat_id: sent.chat_id,
  message_id: sent.message_id,
  text: "Status updated.",
  on_error: "/send-failed"
});
```
{% endcode %}

Keep the chat ID with the message ID. An incoming user's message is not the bot's outgoing reply; copying `request.message_id` from the original command does not identify the reply you just sent.

## Delete a bot message after a delay

Create the following four commands. For each, leave **Answer** and **Keyboard** empty, **Wait for answer** off and the Auto Retry interval empty. This sends one temporary reply, obtains its actual Telegram ID, and schedules removal after 30 seconds.

In `/temporary-message`:

{% code title="/temporary-message" overflow="wrap" %}
```javascript
// Command: /temporary-message
if (!user || !chat || chat.chat_type !== "private") { return; }
Api.sendMessage({
  text: "This message will be removed in about 30 seconds.",
  on_result: "/temporary-sent",
  on_error: "/temporary-error"
});
```
{% endcode %}

In `/temporary-sent`:

{% code title="/temporary-sent" overflow="wrap" %}
```javascript
// Command: /temporary-sent
if (!options || !options.result || !options.result.chat ||
    !options.result.message_id) { return; }
const sent = options.result;
Bot.run({
  command: "/delete-temporary",
  run_after: 30,
  options: {
    chat_id: sent.chat.id,
    message_id: sent.message_id
  }
});
```
{% endcode %}

In `/delete-temporary`:

{% code title="/delete-temporary" overflow="wrap" %}
```javascript
// Command: /delete-temporary
if (!options || !options.chat_id || !options.message_id) { return; }
Api.deleteMessage({
  chat_id: options.chat_id,
  message_id: options.message_id,
  on_error: "/temporary-error"
});
```
{% endcode %}

In `/temporary-error`:

{% code title="/temporary-error" overflow="wrap" %}
```javascript
// Command: /temporary-error
if (!options || typeof options.error !== "string") { return; }
Bot.setProp("temporary_message_error", options.error.slice(0, 300));
```
{% endcode %}

Send `/temporary-message` in your private chat. Expect one bot message, followed by its removal when the scheduled command runs. The incoming command remains. Sending the command twice schedules each returned message ID separately. Manual calls to the callback commands have no result/options and should do nothing.

The `options.chat_id` value here is data for `Api.deleteMessage`, so it is a **Telegram chat ID**. It is not the top-level `Bot.run({chat_id: ...})` field, which takes an **internal Bots.Business chat ID**. The scheduling call preserves its current execution context automatically. Passing both Telegram IDs in `options` identifies the exact message even though the delayed command has no new incoming Telegram request.

Keep the bot running with available iterations. `run_after` is in seconds and queue delays can postpone execution. Telegram permits a bot to delete its outgoing private-chat messages subject to its deletion limits, including the usual 48-hour limit. An already deleted message or a lost permission can cause an error; this example records a bounded diagnostic in the bot property `temporary_message_error` instead of sending another potentially failing message. Clear that diagnostic before a fresh failure test. [Telegram deleteMessage](https://core.telegram.org/bots/api#deletemessage).

## Objects, keyboards, and media

For complete commands that receive a user's photo or document, save its file ID, and send it back, follow [Send photos and documents](../guides/send-media.md). The photo example also adds a URL button.

Supply objects and arrays directly for fields such as `reply_markup`; the BJS wrapper serializes them:

{% code title="Objects, keyboards, and media · Example 11" overflow="wrap" %}
```javascript
Api.sendMessage({
  text: "Need help?",
  reply_markup: {
    inline_keyboard: [[{ text: "Open help", callback_data: "/help" }]]
  }
});
```
{% endcode %}

Create `/help` to handle that callback. For a URL button use `url` instead of `callback_data`. See [inline interactions](inline.md) for handling callback queries and inline mode.

Media methods include `sendPhoto`, `sendAudio`, `sendDocument`, `sendVideo`, `sendVoice`, `sendVideoNote`, `sendAnimation`, and `sendMediaGroup`. Use the method-specific Telegram field, such as `photo` or `document`, and a supported Telegram file ID or accessible URL. A local phone or project file path is not an uploaded Telegram file. To receive photos, documents, contacts or locations from a user, follow [Telegram updates](../guides/telegram-updates.md).

## Supported methods and Api.call

The backend supports established message/media, chat-management, callback/inline-query, payment/game, and command-menu methods. Its allowlist is checked independently of the names present in the BJS wrapper. A new Telegram method appearing in an online example or autocomplete does not prove that this backend accepts it.

`Api.call("sendMessage", {text: "Hello"})` converts a camelCase method name to the backend action name. It follows the same backend restrictions; it does not bypass the allowlist. Prefer named methods for ordinary examples.

## Troubleshooting

For a silent callback, check the exact callback command name and guard against manual execution without `options`. For Telegram errors, check IDs, permissions, markup, and whether the requested method is supported. Use [HTTP](http.md) for non-Telegram web services and [BJS errors](errors.md) for command failures.
