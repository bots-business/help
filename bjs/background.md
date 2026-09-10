---
description: Schedule and cancel delayed BJS commands, configure periodic Auto Retry with an explicit Telegram destination,
  and understand before/after command hooks.
---


# Scheduling and command hooks

Use `Bot.run` to run another command or schedule it after a delay. Use **Auto Retry** for a recurring interval configured in command metadata. Use callbacks for remote results. BJS does not provide a long-lived JavaScript event loop for `setTimeout`, Promises, or background threads.

## A complete reminder

Create `/remind`:

{% code title="/remind" overflow="wrap" %}
```javascript
if (!user || !chat) { return; }
var label = "tea-reminder:" + user.id;
Bot.clearRunAfter({ label: label });
Bot.run({
  command: "/tea-ready",
  run_after: 60,
  label: label,
  options: { text: "Your tea reminder is ready." }
});
Bot.sendMessage("Reminder scheduled. Use /cancel-reminder to cancel it.");
```
{% endcode %}

Create `/tea-ready`:

{% code title="/tea-ready" overflow="wrap" %}
```javascript
if (!options || typeof options.text !== "string") { return; }
Bot.sendMessage(options.text, { parse_mode: null });
```
{% endcode %}

Create `/cancel-reminder`:

{% code title="/cancel-reminder" overflow="wrap" %}
```javascript
if (!user) { return; }
Bot.clearRunAfter({ label: "tea-reminder:" + user.id });
Bot.sendMessage("Pending tea reminder cancelled.");
```
{% endcode %}

`run_after` is in **seconds**. A scheduled request is not a guarantee of execution at an exact wall-clock instant: queue availability, bot state, and limits still apply.

For a reminder that resets when the user returns, follow [remind a user after inactivity](../guides/inactivity-reminder.md). That recipe tracks activity and includes opt-in and cancellation.

## Bot.run options

| Option | Meaning |
| --- | --- |
| `command` | Required target command, optionally followed by text parameters |
| `options` | Structured data available as `options` in the target |
| `run_after` | Positive delay in seconds; without it, the ordinary path calls a subcommand |
| `label` | Label used to find pending scheduled requests for cancellation |
| `user_id`, `chat_id` | Internal Bots.Business IDs for execution context |
| `user_telegramid` | Telegram user ID for a known user |
| `bot_id` | Another bot accessible to the current owner |
| `ignoreMissingCommand` | Relax a missing-command error in the immediate subcommand path |

Changing destination context can cause an automatic one-second scheduled path. The `background` option is not forwarded by the current `Bot.run` wrapper; do not depend on it to remove context or change time limits.

`Bot.runCommand("/next", {step: 2})` is the simpler command-call form. Its result is not a JavaScript return value. Keep command chains short: the backend limits nested command/callback calls and reports “Too many sub commands” when the chain exceeds that limit.

## Cancellation scope

Labels belong to scheduled requests for a bot. Include a user or task identifier when the reminder should be individual. `Bot.clearRunAfter()` without a label clears pending scheduled requests for the bot, including other tasks. Cancelling a pending request does not undo a command that has already executed.

## Auto Retry: run periodically

Auto Retry runs a command's BJS at an interval set in **Auto retry time in seconds**. Each automatic run starts without a current `user`, `chat`, or Telegram `request`, and skips metadata **Answer** and **Keyboard**. The reminder above depends on a user's chat context; do not copy it into Auto Retry unchanged.

### Send an hourly reminder to a fixed chat

Use a test bot and a destination where you intend to receive recurring messages.

1. Create a temporary `/chat-id` command with empty **Answer** and **Keyboard**, and this BJS:

{% code title="Send an hourly reminder to a fixed chat · Example 4" overflow="wrap" %}
```javascript
if (!chat || !chat.chatid) { return; }
Bot.sendMessage(chat.chatid);
```
{% endcode %}

2. Send `/chat-id` in the intended Telegram conversation. For a private chat, start the bot there first; for a group, add the bot and allow it to send messages. Copy the returned Telegram chat ID, including a leading minus sign for a group. It is `chat.chatid`, not the internal `chat.id`.
3. In the mobile app, open the bot's **Commands** and create `/hourly-reminder`. Keep **Answer** and **Keyboard** empty, **Wait for answer** off, and the auto-retry interval empty until the code is ready.
4. Paste the BJS below, replace `YOUR_TELEGRAM_CHAT_ID` with that chat ID, keeping the quotes, and tap **Save**. Configure the destination yourself in the editor; do not let visitors set a shared destination from chat input.

{% code title="/hourly-reminder" overflow="wrap" %}
```javascript
// Command: /hourly-reminder
if (user || chat || request) { return; }
const TARGET_CHAT_ID = "YOUR_TELEGRAM_CHAT_ID";
if (!/^-?[1-9][0-9]*$/.test(TARGET_CHAT_ID)) { return; }
Api.sendMessage({
  chat_id: TARGET_CHAT_ID,
  text: "Your hourly reminder is ready."
});
```
{% endcode %}

5. Open **⋮ → Options**, set **Auto retry time in seconds** to `3600`, and tap **Edit** to save. Ensure the bot is running. On a new command, the first automatic run can happen as soon as the scheduler picks it up; it does not necessarily wait a full interval first.
6. Check the message in the destination and the bot's **Errors**. For a short test, use `60` seconds, observe successive messages, then save `3600` or disable the interval. Remove `/chat-id` when finished.

The first check ignores incoming Telegram messages. It is not authentication for a public endpoint: do not expose this periodic command as a webhook or call it from user-controlled code. `Api.sendMessage` needs the explicit Telegram `chat_id` because automatic runs have no default destination. Use bot properties for shared persistent state; user properties require a user context.

The interval is in seconds, measured from the previous automatic run; it is not a calendar rule such as “every day at 09:00.” Scheduler availability, bot status and limits affect timing. Repeated work consumes iterations, so choose the interval for the task.

### Stop Auto Retry

Open the command's **⋮ → Options**, clear **Auto retry time in seconds** (or set it to `0`), and tap **Edit**. This stops future automatic runs after the setting is saved; it does not undo a run already in progress. `Bot.clearRunAfter` cancels queued `Bot.run` requests, not this metadata interval.

## Always-running commands

The special `@` command provides BJS inserted before a selected command; `@@` provides BJS inserted after it. Use these hooks for small shared checks or setup. They are not independent background jobs.

Example `@` code:

{% code title="Always-running commands · Example 6" overflow="wrap" %}
```javascript
// Only apply this check when a Telegram user exists.
if (user && User.getProp("muted", false) === true) {
  return;
}
```
{% endcode %}

Because the hook and command share an execution body, `return` in `@` prevents the selected command's BJS and later `@@` BJS from running. It does **not** suppress the selected command's metadata **Answer** or **Keyboard**, which are processed separately. For muted-user checks or access restrictions that must also prevent replies, leave those fields empty and send replies from BJS after the check; see [Guard](../libraries/guard.md). A thrown error or an early return in a command can prevent `@@`; it is not a guaranteed `finally` block. Shared hooks must also work for callbacks and triggers without user/chat context.

The `*` command handles unmatched input, including incoming messages without text. It is distinct from the `@` hook. See [receiving Telegram updates](../guides/telegram-updates.md) and [collecting input](../app/collect-input.md).

## Troubleshooting

For a missing reminder, check the label, delay, bot status, target command, and error log. For a missing `user`, compare the original and supplied context. For repeated reminders, confirm whether the scheduling command ran more than once and whether cancellation used the exact label.

Use [broadcast tasks](broadcasts.md) to reach many chats and [HTTP callbacks](http.md) for service responses; neither needs a JavaScript timer loop.
