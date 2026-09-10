---
description: Define reusable SmartBot reply templates, validate numeric input, and understand SmartTasker boundaries.
---


# Build reply templates with SmartBot

`SmartBot`, `SmartAmountDialog` and `SmartTasker` are runtime classes. SmartBot separates reply text and layouts from the commands that supply data. It does not create your product rules or verify that a user earned a reward.

For a complete menu with English and Spanish, shared layouts, and a language switch, follow [Build a two-language SmartBot menu](../guides/smartbot-menu.md). The sections below describe the individual APIs.

## Configure a language template

In an owner-only `/setup` command:

{% code title="Configure a language template · Example 1" overflow="wrap" %}
```javascript
const smart = new SmartBot();
smart.setupLng("en", {
  commands: {
    "/hello": { text: "Hello, {displayName}!", parse_mode: "HTML" }
  },
  titles: {},
  types: {}
});
Bot.sendMessage("Templates saved.");
```
{% endcode %}

Then create `/hello`:

{% code title="Configure a language template · Example 2" overflow="wrap" %}
```javascript
const smart = new SmartBot({ params: { displayName: "friend" } });
smart.handle();
```
{% endcode %}

Expected result: `Hello, friend!`. The first configured language becomes the default. `setupLng` stores the supplied dictionary; re-run your setup after editing it. `setUserLang("en")` selects an already configured language for the current user.

For user-supplied names in HTML templates, escape `<`, `>` and `&`, or choose a suitable plain-text sending path. Placeholders substitute values; they do not sanitize them.

## Organize templates

`commands` maps command names to layouts. A layout can use `text`, `keyboard`, `inline_buttons`, `photo`, `parse_mode`, `chat_id`, `alias`/`aliases`, and editing options. Keep one primary output type per layout rather than combining a photo and several competing output types.

{% code title="Organize templates · Example 3" overflow="wrap" %}
```javascript
const language = {
  commands: {
    "/menu": {
      text: "Choose an action",
      inline_buttons: [[
        { text: "Help", command: "/help" },
        { text: "Website", url: "https://bots.business" }
      ]]
    },
    "/help": { text: "Send /menu to return." }
  },
  types: {},
  titles: {}
};
```
{% endcode %}

Store it with `setupLng` and call `handle()` in the commands that use it. `add(object)` merges template parameters, `set(object)` replaces them, and `fill(textOrObject)` resolves placeholders. Keep `{placeholder}` names and command keys unchanged in translations. Reusable `types` can be referenced with `#/path/to/type`; `titles` adds common parameters.

`run({ command: "/help" })` runs another bot command. Editing a message requires a real message ID and applicable Telegram context; a template's `edit` flag alone cannot identify an arbitrary older message.

`isAlias(message)` checks a supplied string against template aliases and returns true for a match. Supply the text explicitly. These template comparisons are exact and have their own behavior; they are separate from command aliases configured in the app. Constructor options include `strict_params`, `defaultMarkdown`, `skip_cmd_folders` and `debug`. Debug output can contain template parameters, so avoid putting credentials in them.

## Validate an amount

For a complete conversation with a question, invalid-input retry, cancellation, and saved quantity, use [Ask for a quantity](../guides/quantity-dialog.md).

`SmartAmountDialog` validates input but does not transfer or reserve a resource. Without a SmartBot instance it returns **`true` on success or the configured error value on failure**. Compare strictly with `true`:

{% code title="Validate an amount · Example 4" overflow="wrap" %}
```javascript
const dialog = new SmartAmountDialog({
  min: 1, max: 20, curValue: 12, onlyInteger: true,
  dialogErrors: {
    invalid: "Enter a number.",
    notInteger: "Enter a whole number.",
    zero: "No amount is available.",
    notEnough: "More than your available amount.",
    small: "Enter at least 1.",
    big: "Enter no more than 20."
  }
});
const result = dialog.accept("5");
if (result !== true) {
  Bot.sendMessage(result);
  return;
}
Bot.sendMessage("Accepted: " + dialog.amount);
```
{% endcode %}

With `smartBot: smart` supplied, failure returns `false` and the formatted message is in `dialog.errMsg`. The option is `smartBot`, not `smart_bot`. Inputs use digits with an optional decimal point; negative numbers and comma decimals are not accepted. `skipZero` skips the zero **available balance** check, not all numeric limits.

## SmartTasker for task progress

For two complete commands with progress that survives return visits, follow [Save progress in a learning checklist](../guides/task-checklist.md). It is a self-reported learning exercise with no wallet or reward transfers.

Create it with a name, an array of `{ id, amount }` task definitions and your SmartBot instance:

{% code title="SmartTasker for task progress · Example 5" overflow="wrap" %}
```javascript
const smart = new SmartBot();
const tasker = new SmartTasker({
  name: "tutorial", smartBot: smart, balance: 0,
  tasks: [{ id: "read_intro", amount: 1 }]
});
const remaining = tasker.getTasksForWork();
Bot.sendMessage("Tasks remaining: " + remaining.length);
```
{% endcode %}

Use `defineWork(taskId)` and `completeExecution(taskId)` only after your own trusted verification. Completion stores user progress and changes the instance's balance; it does not automatically persist a separate ResourcesLib balance. `manyTimes` permits repeated completion. Task IDs must be strings without spaces or colons.

| Method | Contract |
| --- | --- |
| `defineTask(idOrDefinition)` | Select a task and add its fields to SmartBot parameters |
| `defineWork(taskId)` | Select the task and load its current user execution |
| `skipTask()` | Mark the first remaining task skipped; return whether another task remains |
| `addBalance(amount)` | Change this instance's numeric balance; reject a negative resulting balance |
| `clearUserProgress()` | Clear the current user's saved task progress, allowing a fresh start; reserve for a protected reset/debug flow |
| `prepareTaskQuestion({ taskID, onAnswer })` | Populate the configured question and answer parameters for a template |
| `acceptAnswer(text)` | Parse a callback-shaped value into `{ taskID, isCorrect }`; default to command parameters when omitted |

The question helper `acceptAnswer()` reads correctness from callback parameters; it is not proof of a secure quiz answer. Do not authorize valuable rewards from that flag. For a basic conversation flow, [collect input](../app/collect-input.md) is simpler.
