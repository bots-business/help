---
description: Understand BJS user and chat IDs, Telegram request data, command parameters, callback options, and contexts without a current user.
---

# Context and variables

BJS variables describe the current command execution. Their values depend on how the command was started: a Telegram message, a button, a result callback, a scheduled job, or a Web App request.

## Available variables

| Variable | Meaning and use |
| --- | --- |
| `message` | Command/message text processed by Bots.Business |
| `params` | Text after the command name; for `/hello Alex`, this is `Alex` |
| `options` | Data supplied to a command by `Bot.run`, a result callback, a broadcast task, or a web request |
| `user` | Current user record, when available; includes identifiers and profile fields |
| `chat` | Current chat record, when available |
| `bot` | Current bot; useful fields include its internal `id` and name |
| `request` | Telegram request object relevant to this update; its structure changes with the update type |
| `tgUpdate` | Original Telegram update data, when supplied by the Telegram trigger |
| `content` | Decoded HTTP response body in an HTTP success command |
| `http_status` | HTTP response status in the HTTP callback; convert with `Number(...)` when comparing numerically |
| `http_headers`, `cookies` | Decoded HTTP response metadata |
| `admins`, `owner` | Bot administration context; do not expose these entire objects in public replies |
| `iteration_quota`, `payment_plan` | Execution/account context; use the app's account information for user-facing plan details |

Names are case-sensitive JavaScript identifiers: `Bot` is an API object; `bot` is context data. `User` provides property methods; `user` is the current record. `HTTP` uses uppercase letters, while Telegram methods belong to `Api`.

## Internal IDs and Telegram IDs

| Value | Identifier |
| --- | --- |
| `user.id` | Bots.Business internal user ID |
| `user.telegramid` | Telegram user ID |
| `chat.id` | Bots.Business internal chat ID |
| `chat.chatid` | Telegram chat ID |
| `bot.id` | Bots.Business internal bot ID |

Read each method's contract before passing an ID. `Bot.run({chat_id: ...})` uses an internal chat ID. `Api.sendMessage({chat_id: ...})` uses a Telegram chat ID. Despite its similar name, `Bot.sendMessageToChatWithId` also finds a chat by its Telegram ID. Mixing these values is a common cause of missing destinations.

## Read command parameters

Create `/hello` with this BJS and send `/hello Alex`:

```javascript
var name = String(params || "").trim();
if (!name) {
  Bot.sendMessage("Use /hello followed by your name.");
  return;
}
Bot.sendMessage("Hello, " + name + "!", { parse_mode: null });
```

`params` is text, not an array or a parsed JSON object. Convert deliberately and validate values before using them as quantities or identifiers.

## Pass structured options

Command `/show-menu`:

```javascript
Bot.run({
  command: "/show-section",
  options: { section: "help" }
});
```

Command `/show-section`:

```javascript
if (!options || options.section !== "help") {
  Bot.sendMessage("Open this section from /show-menu.");
  return;
}
Bot.sendMessage("Help: use /hello followed by your name.");
```

Options are a convenient data channel, not proof of authorization. A command that changes privileged data must check the caller or validate a trusted server-side context.

## Handle missing context

A background task or web request may have no `user`, `chat`, or Telegram `request`. Test for the object before reading its fields. `User.getProp` requires a current user; an explicit property lookup through `Bot.getProp({name, user_id})` can target a known internal user ID when appropriate.

For a message update, the incoming message ID is commonly `request.message_id`. For an inline-button callback, the containing message is `request.message`, and its ID is `request.message.message_id`. A callback query's own `request.id` is a different identifier.

If an example works in chat but fails from the app or a timer, inspect only the required context fields on a test bot and compare the trigger. Continue with [Bot methods](bot.md), [properties](user-properties.md), or [HTTP callbacks](http.md).
