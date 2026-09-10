---
description: Limit repeated bot actions with CooldownLib and understand first-call, waiting, and reset behavior.
---

# Limit repeated actions with CooldownLib

Use a cooldown when someone can repeatedly press a button or run a command. Install `CooldownLib` for the bot; it uses the core `Libs.ResourcesLib` compatibility object.

## Wait before allowing another action

For `/check-offer`:

```javascript
Libs.CooldownLib.user.watch({
  name: "offer-check",
  time: 30,
  onStarting: function () {
    Bot.sendMessage("Try this check again in 30 seconds.");
  },
  onWaiting: function (seconds) {
    Bot.sendMessage("Please wait " + Math.ceil(seconds) + " seconds.");
  },
  onEnding: function () {
    Bot.sendMessage("The check is available now.");
    // Perform your permitted action here.
    return true;
  }
});
```

The first call initializes the full waiting period and calls `onStarting`; it does **not** call `onEnding`. When the period has passed, the next call invokes `onEnding`. Returning a truthy value from `onEnding` restarts the period. Returning nothing leaves it expired.

Always supply `onWaiting`. In the checked library, an active cooldown without that callback can fall through to `onEnding`.

## Choose the scope

| Method | Who shares the cooldown |
| --- | --- |
| `Libs.CooldownLib.user.watch(options)` | The current user |
| `Libs.CooldownLib.chat.watch(options)` | Everyone in the current chat |
| `Libs.CooldownLib.watch(options)` | This bot's global cooldown |

Use a different `name` for independent actions. `time` is a positive duration in seconds; changing it reinitializes the cooldown. Callbacks are functions, not BJS command names.

## Inspect a cooldown

```javascript
const resource = Libs.CooldownLib.user.getCooldown("offer-check");
Bot.sendMessage("Seconds left: " + Math.max(0, Math.ceil(resource.value())));
```

The matching `chat.getCooldown(name)` and `getCooldown(name)` read chat and global scope. A cooldown uses elapsed resource growth; it does not schedule a future message. A user must call the command again after waiting.

## When to use another approach

If the first attempt should succeed immediately, implement that explicit first-attempt rule before entering this waiting workflow, or use a timestamp property with a clear initial state. Do not assume `onStarting` and `onEnding` mean the same thing.

Cooldowns reduce repeated actions; they are not authorization checks or an atomic payment guard. Use [Guard](guard.md) for administrator commands and validate any operation's own permissions.
