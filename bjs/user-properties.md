---
description: Save and read BJS properties with User.setProp, Bot.setProp, getProp, and deleteProp; understand automatic types, scope, and persistence.
---

# User and bot properties

Properties keep data between command executions. Use `User` for the current user's progress and `Bot` for settings shared by the bot. A local JavaScript variable does not persist after the command ends.

## Save and read a value

Command `/save-color`:

```javascript
if (!user) { return; }
var color = String(params || "").trim();
if (!color) {
  Bot.sendMessage("Use /save-color followed by a color.");
  return;
}
User.setProp("favorite_color", color);
Bot.sendMessage("Saved. Send /color to read it.");
```

Command `/color`:

```javascript
if (!user) { return; }
var color = User.getProp("favorite_color", "not set");
Bot.sendMessage("Your color: " + color, { parse_mode: null });
```

Use `Bot.setProp("welcome_text", "Welcome!")` and `Bot.getProp("welcome_text", "Hello!")` for a value shared by the bot.

## Delete a saved value

Create `/forget-color`:

```javascript
if (!user) { return; }
User.deleteProp("favorite_color");
Bot.sendMessage("Color removed. Send /color to check it.");
```

Send `/forget-color`, then `/color` in a separate message. The reply should be `Your color: not set`. `Bot.deleteProp(name)` is the equivalent for a shared bot property.

## Methods and types

| Method | Purpose |
| --- | --- |
| `User.setProp(name, value)` | Save a current-user property |
| `User.getProp(name, default_value)` | Read a current-user property |
| `Bot.setProp(name, value)` | Save a bot property |
| `Bot.getProp(name, default_value)` | Read a bot property |
| `User.setProp(name, value, type)`, `Bot.setProp(name, value, type)` | Save with an explicit storage type when needed |
| `User.deleteProp(name)`, `Bot.deleteProp(name)` | Request deletion by writing a null property value |

Use the short methods in new code. They are methods on `User` or `Bot`: a bare `setProp(...)` is not a built-in global function. The longer `setProperty` and `getProperty` names still work and use the same properties. Changing the method name does not require migrating saved data or change its scope.

### When you can omit the type

For an ordinary database property, BJS can infer the storage type from the value: a string becomes `string` (or `text` when longer than 255 characters), a number becomes `integer` or `float`, a Boolean becomes `boolean`, and a plain object becomes `json`. This is why `User.setProp("favorite_color", color)` needs no `"string"` argument.

Chat input remains text until you convert it. For example, `"12"` is stored as text; an omitted type does not turn it into a number. Validate the input, convert it with `Number(...)`, and check the resulting number before saving it.

### When to keep an explicit type

- **Arrays:** use `"json"`, for example `Bot.setProp("tags", ["news", "help"], "json")`. Do not depend on array type inference.
- **Specific storage formats:** keep `"datetime"` for a date string intended as a date property, or another explicit type when preserving an existing storage contract.
- **Memory database:** number and object inference differs from the ordinary database. Keep `"integer"`/`"float"` for numbers and `"json"` for arrays or objects, and verify the saved value in a later command. Explicit types do not remove the scalar-value limitations below.

Supported ordinary types are `integer`, `float`, `boolean`, `string`, `text`, `json`, and `datetime`. The examples below retain `"json"` where it also makes the structured storage explicit. Pass the object itself rather than calling `JSON.stringify` to store it as a string.

Properties are stored by bot, user where applicable, and name. Use separate names for shared settings and personal data, such as `bot:welcome_text` and `user:favorite_color`. The current reader has a limitation when one execution reads the same property name for several users; see the next section before building an administrator command that does this.

## Default values and zero/false

The current property reader uses a truthy fallback for stored scalar values. A saved `0`, `false`, or empty string may therefore produce the supplied default, and may become `undefined` when no default was supplied. Do not use `Bot.getProp("enabled", true)` to distinguish a saved false value from a missing setting.

When this distinction matters, wrap the value in a JSON object:

```javascript
// Save in one command.
Bot.setProp("feature", { enabled: false, count: 0 }, "json");
```

```javascript
// Read in a later command.
var feature = Bot.getProp("feature", { enabled: false, count: 0 });
Bot.sendMessage(feature.enabled ? "Feature enabled" : "Feature disabled");
```

The object is truthy while its `enabled` and `count` fields keep their original values. Do not infer durable behavior only from reading a property immediately after writing it in the same execution; that read may use a temporary in-memory value.

## Object form and another user

An explicit lookup is useful when a command already knows the intended internal user ID:

```javascript
// Read your own score with the same explicit form used for another known user.
if (!user) { return; }
var score = Bot.getProp({
  name: "score",
  user_id: user.id,
  default_value: 0
});
Bot.sendMessage("Score: " + score);
```

The write form accepts `name`, `value`, `type`, `user_id`, `user_telegramid`, `bot_id`, and `list`. `user_id` is a Bots.Business ID; `user_telegramid` is a Telegram ID. Writes for another bot are subject to backend access checks. Memory-database writes do not support `user_telegramid`; use a known internal user ID for that mode.

`User.getProp` still requires a current `user`, even with an options object. With object-form reads, supply `user_id` explicitly; do not assume the short-form scope will be inserted into an arbitrary object. See [context and identifiers](context.md).

### Read and write another known user's property

Use a test bot and a user who has already opened it in Telegram. These commands are administrator tools: follow [trusted administrator setup](security.md#restrict-a-command-to-a-trusted-telegram-user), and replace `YOUR_TELEGRAM_USER_ID` yourself in every command below. Keep **Answer** and **Keyboard** empty, **Wait for answer** off, and Auto Retry empty.

To obtain the IDs without guessing, temporarily create `/scope-ids`:

```javascript
// Command: /scope-ids
if (!user) { return; }
Bot.sendMessage("Bot ID: " + bot.id + "\nUser ID: " + user.id);
```

Ask the intended test user to send `/scope-ids` in a private conversation with this bot and give you their **User ID**. Replace `TARGET_BB_USER_ID` in both commands below, keeping the quotes. This is the internal `user.id`, not `user.telegramid`. Configure the destination in the editor; do not accept an arbitrary user ID from visitors.

Create `/set-user-note`:

```javascript
// Command: /set-user-note
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const targetUserId = Number("TARGET_BB_USER_ID");
if (!Number.isSafeInteger(targetUserId) || targetUserId < 1) {
  Bot.sendMessage("Set TARGET_BB_USER_ID in the command code.");
  return;
}
Bot.setProp({
  name: "user:" + targetUserId + ":support_note",
  user_id: targetUserId,
  value: "Follow up tomorrow."
});
Bot.sendMessage("Note saved. Send /read-user-note to check it.");
```

Create `/read-user-note`, then run it in a **separate message** after saving:

```javascript
// Command: /read-user-note
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const targetUserId = Number("TARGET_BB_USER_ID");
if (!Number.isSafeInteger(targetUserId) || targetUserId < 1) {
  Bot.sendMessage("Set TARGET_BB_USER_ID in the command code.");
  return;
}
const note = Bot.getProp({
  name: "user:" + targetUserId + ":support_note",
  user_id: targetUserId,
  default_value: "not set"
});
Bot.sendMessage("Support note: " + note, { parse_mode: null });
```

Expect `Support note: Follow up tomorrow.` The `user_id` gives the property its user scope even though the call uses `Bot`. The name also includes that ID to keep different users' notes distinct in the current reader. Ordinary-database writes to another user require that user to be known to the calling bot. This does not grant that user administrator access.

### Read and write another bot's property

Use two test bots owned by the **same Bots.Business account**. The first bot runs the commands; the second stores the shared setting. Run the temporary `/scope-ids` command in the second bot and copy its **Bot ID**. Replace `TARGET_BB_BOT_ID` and the administrator ID in both commands below. A bot ID is not its Telegram username or token.

Create `/set-other-bot-notice` in the first bot:

```javascript
// Command: /set-other-bot-notice
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const targetBotId = Number("TARGET_BB_BOT_ID");
if (!Number.isSafeInteger(targetBotId) || targetBotId < 1) {
  Bot.sendMessage("Set TARGET_BB_BOT_ID in the command code.");
  return;
}
Bot.setProp({
  name: "bot:shared_notice",
  bot_id: targetBotId,
  value: "Service opens at 09:00."
});
Bot.sendMessage("Notice saved. Send /read-other-bot-notice to check it.");
```

Create `/read-other-bot-notice` in the first bot, and run it after the write has finished:

```javascript
// Command: /read-other-bot-notice
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || String(user.telegramid) !== ADMIN_TELEGRAM_ID) { return; }
const targetBotId = Number("TARGET_BB_BOT_ID");
if (!Number.isSafeInteger(targetBotId) || targetBotId < 1) {
  Bot.sendMessage("Set TARGET_BB_BOT_ID in the command code.");
  return;
}
const notice = Bot.getProp({
  name: "bot:shared_notice",
  bot_id: targetBotId,
  default_value: "not set"
});
Bot.sendMessage("Shared notice: " + notice, { parse_mode: null });
```

Expect `Shared notice: Service opens at 09:00.` The read object also accepts `other_bot_id`, but use `bot_id` consistently for these reads and writes. The backend checks the calling bot owner's access to the destination; knowing another bot's ID does not grant access. The Telegram administrator check protects the command from visitors and is separate from this account-level check.

These examples use strings and explicit internal IDs. Use the same storage mode for both bots: the executing bot selects ordinary-database or memory-database access, so changing `bot_id` does not fetch data from the other storage system. Memory writes do not support `user_telegramid`; use an internal `user_id`. Keep explicit numeric/JSON types where described above, and always verify with a later execution. Remove the temporary ID command when finished.

### Reading several users in one execution

The current runtime can return one user's value for several different `user_id` lookups when those lookups share a property name. For example, two saved `score` properties can both read as the second user's score in the same execution. Immediate reads after `User.setProp` for several users with the same name can also return the first pending value for both users. Do not use these patterns to authorize access or calculate balances.

For data already saved in the ordinary database, explicit `user_telegramid` reads avoid this particular internal-ID lookup cache. Read after the saving execution has finished, and use a Telegram ID that your command is authorized to inspect. This example reads the current user's saved value through that form:

```javascript
if (!user) { return; }
var score = Bot.getProp({
  name: "score",
  user_telegramid: user.telegramid,
  default_value: 0
});
Bot.sendMessage("Saved score: " + score);
```

For new cross-user data, another option is to include the internal user ID in the name, such as `"user:" + targetUserId + ":score"`, and still specify the intended `user_id`. This keeps the names different within one execution. Use a separate `bot:` prefix for shared settings. A naming change requires migrating existing data; it does not rename old properties automatically. These workarounds do not turn read-modify-write operations into atomic transactions, and the Telegram-ID read workaround is not a promise about memory-database mode.

## User groups and lists

`User.addToGroup(name)` stores the user's single `group` string. `User.getGroup()` reads it; `User.removeGroup()` clears it. Adding a new group replaces the previous value. This does not create a Telegram group or a multiple-membership collection. Use [Lists](lists.md) when you need several memberships or paginated data.

## Troubleshooting

An undefined value usually means a wrong name/scope, no saved property, or a false-like scalar value. An error mentioning `User.getProperty` can still come from `User.getProp`, because the short name calls the same implementation. “User is not defined” means the trigger lacks a user; supply an appropriate context or use an explicit supported lookup. Avoid using ordinary read-modify-write properties as an atomic balance system: simultaneous executions can read the same old value.

Use the [Properties screen](../app/properties.md) to inspect test data, and [Admin Panel](admin-panel.md) for settings an owner should edit through a form.
