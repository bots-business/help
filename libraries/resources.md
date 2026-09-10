---
description: Store numeric resources, choose user or chat scope, and configure growth with the current ResLib contract.
---

# Manage resources with ResLib

Use `ResLib` for numeric resources with time based growth, such as energy in a game. For a simple counter without growth, [a user property](../bjs/user-properties.md) is often enough. The runtime provides `ResLib`; no Store installation is required.

## Choose the resource owner

| Expression | Scope |
| --- | --- |
| `ResLib.userRes("energy")` | Current Telegram user, across their chats with this bot |
| `ResLib.chatRes("energy")` | Current chat |
| `ResLib.anotherUserRes("energy", telegramId)` | Another user's **Telegram ID** |
| `ResLib.anotherChatRes("energy", chatId)` | The supplied chat ID; a fixed string can name a bot wide resource |

Resource names are case sensitive. `energy` and `Energy` are different resources. The older `Libs.ResourcesLib` name refers to the same core implementation.

## Initialize once, then read and spend

For a test command `/energy-setup`:

```javascript
if (User.getProp("energyInitialized")) {
  Bot.sendMessage("Energy is already initialized.");
  return;
}
ResLib.userRes("energy").set(10);
User.setProp("energyInitialized", true, "boolean");
Bot.sendMessage("You have 10 energy.");
```

For `/use-energy`:

```javascript
const energy = ResLib.userRes("energy");
if (!energy.have(2)) {
  Bot.sendMessage("You need 2 energy.");
  return;
}
energy.remove(2);
Bot.sendMessage("Energy remaining: " + energy.value());
```

`set(number)` replaces the value; `add(number)` increases it. `have(amount)` returns false for zero, negative amounts or insufficient resources. `remove(amount)` throws if there is not enough; `removeAnyway(amount)` can take the resource below zero. Pass numbers, not numeric strings, and validate user input first.

## Add growth

Run this setup once, rather than resetting it whenever the user asks for their balance:

```javascript
const energy = ResLib.userRes("energy");
energy.set(10);
energy.growth.add({ value: 1, interval: 60, max: 100 });
```

Read `energy.value()` to include elapsed growth. Growth is calculated when the resource is read; it does not execute your command or send a message every minute. `baseValue()` reads the stored base amount.

For percentages the parameter is **`percent`**, not `value`:

```javascript
const score = ResLib.userRes("trainingScore");
score.set(100);
score.growth.addPercent({ percent: 5, interval: 3600 });
```

`addPercent` uses the growth base value; `addCompoundInterest` uses compounding. Both take `percent` and `interval` in seconds. Optional `max_iterations_count` limits growth periods. Use `growth.info()`, `growth.isEnabled()`, `growth.stop()`, `growth.resume()` and `resetGrowth()` to inspect or manage the configuration.

For an existing configured growth schedule, `growth.title()` describes it, `growth.progress()` returns the percentage elapsed toward the next period, and `growth.willCompletedAfter()` estimates seconds until that period. Check that growth exists and is enabled before using these display helpers; they do not start a timer or call a command. `resetGrowth()` removes the schedule and clears its enabled marker.

The implementation treats `min` and `max` as truthy values. In particular, do not rely on `min: 0` alone to enforce a zero lower bound for decreasing growth; explicitly check or clamp the resulting value where required.

## Transfers and limits

`source.transferTo(destination, amount)` transfers between resources with the same name. `source.exchangeTo(destination, { remove_amount: 2, add_amount: 5 })` supports different names/amounts. These operations perform multiple reads and writes; they are not a guaranteed atomic financial ledger. Repeated or simultaneous requests require a separate duplicate and concurrency design.

`destination.takeFromAnother(source, amount)` expresses the same-name transfer from the receiver's side. The `transferToAnyway` and `takeFromAnotherAnyway` variants skip the sufficient-balance check and can leave the source below zero. Use them only when a negative resource is intentional.

Unexpected values usually come from resetting in `/start`, choosing the wrong scope, mixing Telegram IDs with internal BB IDs, or using an old percentage example. For restricting repeated actions, see [Cooldowns](cooldown.md).
