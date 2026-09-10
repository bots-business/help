---
description: Learn JavaScript basics, remember a name, validate a quantity, and keep
  a simple learning checklist in a Bots.Business test bot.
cover: ../.gitbook/assets/cover-data-dialogs.webp
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Data and dialogs

**Your result:** a bot that remembers a name, accepts only a valid quantity, and keeps a self-reported learning checklist between commands.

**Before you start:** use a working test bot with a private Telegram chat. You will create and edit BJS commands. The dialog and checklist helpers used here are included in the runtime. Complete [First bot and menu](first-bot.md) if the bot does not reply yet.

{% hint style="warning" %}
Check existing command names before pasting a recipe. Both dialog examples use `/cancel`. Keep one shared cancellation command: when moving from the name example to quantity, replace the name-only cancellation Answer with a neutral message such as `Cancelled. Send /name or /quantity to start again.` Keep its Wait for answer off and its BJS empty. Do not leave the old Answer and add a second BJS reply, or cancellation can send two messages. The known command itself cancels the pending wait.
{% endhint %}

{% stepper %}
{% step %}
### Learn the JavaScript you will use

**Prepare:** choose a practice command that does not perform real payments or change another user's data.

**Do:** work through [JavaScript basics for BJS beginners](../bjs/javascript-basics.md), including variables, conditions, loops, functions, and the `/quote` example.

**Check:** send the practice command and compare its reply with the example. Change an input value and explain why the result changes. Read [What survives the next message?](../bjs/javascript-basics.md#what-survives-the-next-message) before continuing.

**Next:** store a value supplied by the person chatting with the bot.
{% endstep %}
{% step %}
### Ask for a name

**Prepare:** create `/name` and configure its Answer and Wait for answer exactly as the guide describes. Add the shared `/cancel` command described above.

**Do:** follow [Ask a question and collect the reply](../app/collect-input.md). Keep the full code in that guide as your source.

**Check:** `/name` asks a question first; replying with a name runs the BJS and stores `display_name`. Inspect it in [User properties](../app/properties.md#find-a-property), then start another question and send `/cancel` to check that cancellation does not save it as a name.

**Next:** add validation instead of accepting any text.
{% endstep %}
{% step %}
### Accept a valid quantity

**Prepare:** `SmartAmountDialog` is included in the runtime; no Store installation is needed. Keep the shared cancellation command. Do not add another `/cancel` with the same name.

**Do:** follow [Ask for a quantity and validate the reply](../guides/quantity-dialog.md), using its `/quantity` command and its stated minimum and maximum.

**Check:** run the guide's whole test conversation: valid number, invalid text, out-of-range input, and cancellation. Confirm that `demo_quantity` changes only after a valid answer and that an invalid answer lets you try again.

**Next:** save progress across return visits.
{% endstep %}
{% step %}
### Keep a learning checklist

**Prepare:** `SmartTasker` and `SmartBot` are included in the runtime. Use the private-chat setup from [the checklist recipe](../guides/task-checklist.md). Check that `/learn` and `/learn-done` are available names in the test bot.

**Do:** follow [Save progress in a learning checklist](../guides/task-checklist.md), keeping its task names and the two command examples together.

**Check:** open `/learn`, mark a task, then open `/learn` again. Follow the recipe's repeated-completion and unknown-task tests too. The checklist records the user's claim; it does not automatically verify that a lesson was completed.

**Next:** [connect external services](integrations.md), or choose a [data recipe](../guides/recipes.md#user-data-and-access).
{% endstep %}
{% endstepper %}

{% hint style="info" %}
<img src="../.gitbook/assets/mel-02-help-menu-mobile.webp" alt="" width="64">

**Mel’s tip**

A JavaScript variable lasts for the current execution. Use the property saved by the recipe when the next message needs the value. Test a second command or return visit so you check persistence, not just the current reply.
{% endhint %}

## Finish the route

Check all three flows again in the same bot. `/cancel` must work for either question; the stored name, quantity, and checklist must remain separate. If a known command unexpectedly answers a question, review [waiting for a reply](../app/collect-input.md#validation-and-repeated-questions).
