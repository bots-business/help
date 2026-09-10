---
description: Send an optional reminder after a user stops interacting with your Telegram bot, reset the delay on new activity, and let the user cancel.
---

# Remind a user after inactivity

This recipe sends one reminder after an opted-in user stops sending private messages or pressing inline buttons. Each new interaction starts the waiting period again. It uses a scheduled command per user; it does not scan every user in a daily loop.

Start with **60 seconds** on a test bot. After testing, change the delay to `86400` for 24 hours. The user must have started the bot in a private chat.

## 1. Track incoming activity

Create a command named `@`, or merge this code into your existing `@` hook. Keep its Answer and Keyboard empty. This function returns from its own body so that unrelated commands can continue running.

```javascript
// Command: @
function trackPrivateActivity() {
  if (!user || !chat || chat.chat_type !== "private") { return; }
  // Count the incoming interaction once, not its internal callbacks.
  if (completed_commands_count !== 0) { return; }
  const incoming = tgUpdate && (tgUpdate.message || tgUpdate.callback_query);
  if (!incoming || !incoming.from || incoming.from.is_bot) { return; }
  if (String(incoming.from.id) !== String(user.telegramid)) { return; }

  const state = User.getProp("inactivityReminder");
  if (!state || state.enabled !== true) { return; }

  const delaySeconds = 60;
  const now = Date.now();
  const interactionId = incoming.message_id || incoming.id;
  const token = String(interactionId) + ":" + now;
  User.setProp("inactivityReminder", {
    enabled: true,
    token: token,
    lastActiveAt: now,
    delaySeconds: delaySeconds,
    chatId: chat.chatid
  }, "json");
  const label = "inactivity:" + user.id;
  Bot.clearRunAfter({ label: label });
  Bot.run({
    command: "/inactivity-reminder",
    run_after: delaySeconds,
    label: label,
    options: { activityToken: token }
  });
}
trackPrivateActivity();
```

`tgUpdate` is the incoming Telegram update. Scheduled runs and HTTP callbacks should not count as new user activity. The command counter excludes follow-up BJS calls from the same interaction. Group messages are deliberately excluded.

The hook runs when BJS executes. **Add BJS to every command whose activity should reset the reminder.** A command containing only metadata Answer can reply without running `@`; it also matches its own name rather than falling back to `*`. For an Answer-only command, keep its Answer and save this harmless line in its BJS editor:

```javascript
// BJS for an otherwise Answer-only command, such as /help.
void 0;
```

Create a master command `*` to catch ordinary text and non-text messages that do not match another command. Give it the same `void 0;` BJS if it needs no other behavior. If `*` already handles contacts or other events, retain that code. See [Telegram updates](telegram-updates.md).

## 2. Let the user enable and disable reminders

Create `/reminders-on` with empty Answer and Keyboard and Wait for answer off:

```javascript
// Command: /reminders-on
if (!user || !chat || chat.chat_type !== "private") { return; }
User.setProp("inactivityReminder", { enabled: true }, "json");
trackPrivateActivity();
Bot.sendMessage("Reminder enabled. Send /reminders-off to stop it.");
```

`trackPrivateActivity` is defined by the `@` hook, so install that hook first. Enabling starts the first waiting period immediately.

Create `/reminders-off`:

```javascript
// Command: /reminders-off
if (!user || !chat || chat.chat_type !== "private") { return; }
User.deleteProp("inactivityReminder");
Bot.clearRunAfter({ label: "inactivity:" + user.id });
Bot.sendMessage("Inactivity reminders disabled.");
```

## 3. Send only if the user is still inactive

Create `/inactivity-reminder` with empty Answer and Keyboard, Wait for answer off, and no Auto Retry interval:

```javascript
// Command: /inactivity-reminder
if (!user || !options || typeof options.activityToken !== "string") { return; }
const state = User.getProp("inactivityReminder");
if (!state || state.enabled !== true || state.token !== options.activityToken) { return; }
if (state.remindedToken === state.token) { return; }
const elapsed = Date.now() - state.lastActiveAt;
if (!Number.isFinite(elapsed) || elapsed < state.delaySeconds * 1000) { return; }

// Record the attempt before sending. Do not retry indefinitely if blocked.
state.remindedToken = state.token;
User.setProp("inactivityReminder", state, "json");
Api.sendMessage({
  chat_id: state.chatId,
  text: "Ready to continue? Send /start whenever you like."
});
```

The saved token lets an older queued run detect that a more recent interaction replaced it. The attempt marker avoids sending again when the same completed reminder is run sequentially. Delivery still depends on the scheduler and Telegram; simultaneous executions do not provide an exactly-once guarantee.

After a reminder, a new interaction starts another waiting period. `/reminders-off` disables this behavior. Telegram can reject delivery if the user blocks the bot; inspect the error rather than starting an endless retry loop.

## 4. Check the complete flow

1. Send `/reminders-on` in a private chat. Stay silent for at least 60 seconds and check that one reminder arrives.
2. Send another message, wait about 30 seconds, then send a second one. The reminder should wait for the full interval after that second interaction. Also test a formerly Answer-only command after adding its BJS line.
3. Send `/reminders-off` during the waiting period. No pending reminder should arrive afterward.
4. Type `/inactivity-reminder` manually. It should send nothing because there is no scheduled `options.activityToken`.
5. Test a second Telegram user: enabling or cancelling one user's reminder must not change the other's.
6. Change `delaySeconds` in `@` to `86400` when ready. Existing pending reminders keep their previous settings until another interaction replaces them.

This hook observes interactions for which BJS executes. An initial **Wait for answer** prompt can be sent before the command's BJS runs, and cached commands may skip it. For those flows, arrange an explicit uncached BJS activity step or define the reminder around completion of the reply. Do not cache the tracking or reminder commands. See [collect input](../app/collect-input.md), [caching](../bjs/caching.md) and [scheduling](../bjs/background.md).
