---
description: Build BJS inline keyboards, handle callback-query data, and answer Telegram inline searches through the special
  /inlineQuery command.
---


# Inline buttons and inline mode

Inline buttons belong to a bot message. Telegram inline mode lets someone type a bot's username and a query in another chat. They are separate features with different request objects.

## Attach a callback button

Create `/menu`:

{% code title="/menu" overflow="wrap" %}
```javascript
Api.sendMessage({
  text: "Choose a section:",
  reply_markup: {
    inline_keyboard: [[
      { text: "Help", callback_data: "/section help" },
      { text: "Website", url: "https://bots.business/" }
    ]]
  }
});
```
{% endcode %}

Create `/section`:

{% code title="/section" overflow="wrap" %}
```javascript
if (!request || !request.id || !request.data) { return; }
Api.answerCallbackQuery({ callback_query_id: request.id });
if (params !== "help") {
  Bot.sendMessage("Unknown section.");
  return;
}
Bot.sendMessage("Send /menu to return to the menu.");
```
{% endcode %}

Bots.Business uses the callback data as command text, so `/section help` selects `/section` with `params` equal to `help`. Keep callback values small; Telegram limits `callback_data` to 1–64 bytes. Store larger state in properties and pass a short reference. [Telegram inline keyboard reference](https://core.telegram.org/bots/api#inlinekeyboardbutton).

Answering the query dismisses the button's loading state. The backend may also acknowledge successful button handling automatically; call `answerCallbackQuery` yourself when you need predictable acknowledgement or a custom notification.

## Which ID belongs where?

| Value | Meaning |
| --- | --- |
| `request.id` | Callback-query ID, passed to `answerCallbackQuery` |
| `request.data` | Button command/data |
| `request.message.message_id` | Message containing the button, when supplied |
| `request.message.chat.id` | Telegram chat of that message |
| `request.inline_message_id` | Identifier for a message sent through inline mode, when applicable |

Do not assume every callback has `request.message`. Inline-mode messages can use `inline_message_id` instead. Use the matching Telegram method fields when editing the message.

Callback data is input, not authorization. A button labelled “Admin” does not restrict who can trigger its command. Check the caller before changing protected data.

## Answer inline searches

Enable inline mode for the bot in BotFather, then create the exact special command `/inlineQuery`.

{% code title="/inlineQuery" overflow="wrap" %}
```javascript
// Command: /inlineQuery
if (!request || !request.id) { return; }
var query = (request.query || "").trim();
var text = query ? "You searched for: " + query : "Hello from this bot!";
Api.answerInlineQuery({
  inline_query_id: request.id,
  results: [{
    type: "article",
    id: "help-result",
    title: query ? "Send your search text" : "Send a greeting",
    input_message_content: { message_text: text }
  }],
  cache_time: 0,
  is_personal: true
});
```
{% endcode %}

Type `@YourBotUsername hello` in a Telegram message field, then choose the result. `request.query` is the inline search text; `request.id` identifies that search. Answer with `Api.answerInlineQuery`, not an ordinary chat reply. Choose sensible caching only after verifying the result is safe to reuse.

This example returns plain text. Validate and escape user input if you later add formatting, URLs, or richer content. See Telegram's [inline result formats](https://core.telegram.org/bots/api#inlinequeryresult).

## Troubleshooting

If buttons send unexpected commands, compare `callback_data` and the saved command names. If inline search is absent, check BotFather's inline-mode setting and the special `/inlineQuery` name. If a query fails, inspect its request shape, response fields, and [API error handling](telegram-api.md).

For simpler Bots.Business keyboard helpers see [Bot methods](bot.md); for buttons below the message input see [reply keyboards](../app/reply-keyboard.md).
