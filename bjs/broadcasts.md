---
description: Create BJS broadcasts with Bot.runAll, choose eligible chat types, handle task creation, and avoid recursive
  or unsupported broadcast actions.
---


# BJS broadcasts with Bot.runAll

`Bot.runAll` creates a task that runs one command for each eligible chat. Use the [Broadcasts screen](../app/broadcasts.md) for ordinary campaigns; use BJS when each recipient needs command logic or different content.

## A complete broadcast flow

Create `/send-announcement` with empty **Answer** and **Keyboard**, and **Wait for answer** off. Follow [trusted administrator setup](security.md#restrict-a-command-to-a-trusted-telegram-user) to obtain your own Telegram user ID, then replace `YOUR_TELEGRAM_USER_ID` below in the mobile editor. This check must pass before a real task is created:

{% code title="A complete broadcast flow · Example 1" overflow="wrap" %}
```javascript
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || !user.telegramid ||
    String(user.telegramid) !== ADMIN_TELEGRAM_ID) {
  return;
}
Bot.runAll({
  command: "/announcement-item",
  for_chats: "private-chats",
  options: { text: "Our new help pages are available." },
  on_create: "/announcement-created"
});
```
{% endcode %}

The unchanged placeholder refuses every caller. Account linking is not required for this explicit ID check. Create `/announcement-item`:

{% code title="A complete broadcast flow · Example 2" overflow="wrap" %}
```javascript
if (!options || !options.task || typeof options.text !== "string") { return; }
Bot.sendMessage(options.text, { parse_mode: null });
```
{% endcode %}

Create `/announcement-created`:

{% code title="/announcement-created" overflow="wrap" %}
```javascript
if (!options || !options.run_all_task) { return; }
Bot.sendMessage("Broadcast task created: " + options.run_all_task.id);
```
{% endcode %}

Run the campaign only after checking it on a test bot with a small intended audience. Task creation is not a delivery confirmation.

## Options and recipient context

| Option | Meaning |
| --- | --- |
| `command` | Required recipient command |
| `for_chats` | `all`, `private-chats`, `group-chats`, or `super-group-chats` |
| `options` | Data supplied to the recipient command |
| `on_create` | Command called after task creation with `options.run_all_task` |

Inside the recipient command, `chat` is the current recipient, `user` is the chat's associated user when available, and `options.task` identifies the task context. A group may not have the same user context as a private chat. Do not reuse a single caller's user ID for every recipient.

The task mechanism excludes unavailable/blocked destinations according to its eligibility rules. Review task progress, exclusions, and delivery details in the app rather than interpreting the number of known chats as guaranteed deliveries.

## Restrictions

The recipient command must not use `Bot.run`/`Bot.runCommand` or `HTTP`: the backend explicitly rejects those actions inside `runAll`. Prepare shared HTTP data before creating the task and pass the needed values in `options`, or load appropriate saved properties in the recipient command.

Do not broadcast a command that creates the same broadcast again. Keep message rendering within the recipient command, and avoid unbounded loops or repeated task creation from a user-accessible command.

For media, use a supported `Api.sendPhoto`, `Api.sendDocument`, or another suitable method in the recipient command. Prepare the media ID/URL and validate the example with a small audience first. The ordinary [Telegram API](telegram-api.md) permission and parameter rules still apply.

## Read a task

When you have a task's internal ID, `new RunAllTask({id: taskId})` loads its information. Its basic `status` values include `new`, `in progress`, `paused`, `terminated`, and `finished`. Do not calculate delivery success from status alone; a finished task can include excluded or failed recipients.

## Troubleshooting

If the task is created but nothing is sent, inspect the recipient command, eligible audience, bot quota, task state, and errors. If a method is refused, check the restrictions above. If the task sends multiple times, inspect which trigger created it and add a deliberate campaign-start guard.

Related: [command scheduling](background.md), [Bot methods](bot.md), and [security](security.md).
