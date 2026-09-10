---
description: Write Bots.Business commands with BJS, understand execution and callbacks, and choose the right API for messages, data, and integrations.
---

# Coding with BJS

BJS is the JavaScript environment used by Bots.Business commands. Use it when a command needs conditions, saved data, buttons, an API request, or another action that a fixed Answer cannot provide.

You write the body of a command in the mobile [command editor](../app/commands.md). Bots.Business supplies objects such as `Bot`, `User`, `Api`, and `HTTP`; you do not import them.

New to programming? Start with [JavaScript basics for BJS beginners](javascript-basics.md): variables, conditions, loops, functions, and a small command you can build yourself.

## Your first BJS command

Create a command named `/hello`, open its BJS editor, save this code, then send `/hello` to your bot:

```javascript
var name = user && user.first_name ? user.first_name : "there";
Bot.sendMessage("Hello, " + name + "!", { parse_mode: null });
```

The bot replies in the current chat. `user` describes the person in this execution; `Bot.sendMessage` requests an outgoing message. Plain-text mode prevents a name containing formatting characters from changing the message.

## How execution works

1. A message, callback, scheduled command, or another supported trigger selects a command.
2. Its BJS code runs with that trigger's context.
3. Calls such as `Bot.sendMessage` and `HTTP.get` create actions for Bots.Business to process.
4. A result callback starts another named command. Read its result from the appropriate callback variables.

Do not assign an HTTP or Telegram action to a variable and expect the remote response immediately. [HTTP](http.md) returns response data to a `success` command; [Telegram API](telegram-api.md) uses `on_result` and `on_error`.

Local variables are for one execution. Save information needed by a later message in [properties](user-properties.md). Calling another command does not make the current execution wait for a JavaScript return value.

## Choose a reference

| Task | Start here |
| --- | --- |
| Understand `user`, `chat`, `request`, `params`, and `options` | [Context and variables](context.md) |
| Send/edit messages, add keyboards, or call another command | [Bot methods](bot.md) |
| Save settings or a user's progress | [User and bot properties](user-properties.md) |
| Use Telegram methods and result callbacks | [Telegram API](telegram-api.md) |
| Call an external HTTP service | [HTTP requests](http.md) |
| Create settings editable in the app | [Admin Panel](admin-panel.md) |
| Work with lists of properties or users | [Lists](lists.md) |
| Return JSON to a web request; understand the current HTML restriction | [Web App](web-app.md) |
| Schedule work or reuse command code | [Background work and command hooks](background.md) |
| Send a command to multiple chats | [BJS broadcasts](broadcasts.md) |
| Respond to inline queries or button clicks | [Inline interactions](inline.md) |
| Reuse a stable response | [Caching](caching.md) |
| Diagnose a failed command | [BJS errors](errors.md) |

## Language and limits

Use ordinary JavaScript expressions, functions, arrays, objects, conditions, and bounded loops. This environment does not provide a browser DOM, Node.js modules, or a general asynchronous application process. `require`, top-level `await`, Promises, `queueMicrotask`, the `Function` constructor, Proxy construction, and WebAssembly are not available for ordinary BJS use. Use [command scheduling](background.md) instead of timers.

Execution has time, memory, and action limits. Keep a command focused and avoid a loop that makes one request for every user. Use Lists pagination and the broadcast task mechanism where appropriate.

## If your command does not reply

Check that the command was saved, the bot is running, and the exact command name matches the message. Open the bot's [error log](errors.md), then test a single `Bot.sendMessage("Hello")` before restoring more code. A command triggered without a chat may need an explicit destination or a non-message result, such as `WebApp.render`.

Next: [understand the execution context](context.md).
