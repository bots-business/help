---
description: Build a small learning checklist with SmartTasker, validate task IDs, save completion per user and show the remaining
  tasks on a later visit.
---


# Save progress in a learning checklist

This bot lets a user mark two learning steps as done and come back to their remaining tasks. It is a self-reported checklist: tapping a button records the user's choice, not proof of an external action.

{% hint style="info" %}
`SmartTasker` and `SmartBot` are included in the runtime. Run this example in a private chat. Create both commands with empty Answer and Keyboard and **Wait for answer** off.
{% endhint %}

{% stepper %}

{% step %}

### Show the remaining tasks

Create `/learn`:

{% code title="/learn" overflow="wrap" %}
```javascript
// Command: /learn
if (!user || !chat || chat.chat_type !== "private") { return; }
var tasks = [
  { id: "intro", title: "Read the introduction", amount: 1 },
  { id: "first-bot", title: "Create a test bot", amount: 1 }
];
var tasker = new SmartTasker({
  name: "help-checklist-v1",
  tasks: tasks,
  smartBot: new SmartBot(),
  balance: 0
});
var remaining = tasker.getTasksForWork();
var done = tasks.length - remaining.length;
if (remaining.length === 0) {
  Api.sendMessage({ text: "All " + tasks.length + " learning steps are done." });
  return;
}
Api.sendMessage({
  text: "Learning checklist: " + done + "/" + tasks.length +
    " done. Tap a step after you complete it.",
  reply_markup: {
    inline_keyboard: remaining.map(function (task) {
      return [{ text: "Done: " + task.title, callback_data: "/learn-done " + task.id }];
    })
  }
});
```
{% endcode %}

The buttons send only a task ID. Task titles and weights are defined in your code. `amount: 1` satisfies SmartTasker's required numeric task weight; this recipe does not credit a resource balance or make payments.

{% endstep %}

{% step %}

### Record completion

Create `/learn-done` with the same task definitions and checklist name:

{% code title="/learn-done" overflow="wrap" %}
```javascript
// Command: /learn-done
if (!user || !chat || chat.chat_type !== "private") { return; }
if (request && typeof request.id === "string" && typeof request.data === "string") {
  Api.answerCallbackQuery({ callback_query_id: request.id });
}
var tasks = [
  { id: "intro", title: "Read the introduction", amount: 1 },
  { id: "first-bot", title: "Create a test bot", amount: 1 }
];
var taskId = String(params || "").trim();
if (!tasks.some(function (task) { return task.id === taskId; })) {
  Api.sendMessage({ text: "Unknown learning step. Open /learn." });
  return;
}
var tasker = new SmartTasker({
  name: "help-checklist-v1",
  tasks: tasks,
  smartBot: new SmartBot(),
  balance: 0
});
var execution = tasker.completeExecution(taskId);
Api.sendMessage({
  text: execution ? "Step saved." : "This step was already marked done."
});
Bot.runCommand("/learn");
```
{% endcode %}

`completeExecution` stores the user's progress. With these task definitions, repeating the same task does not complete it a second time. The next `/learn` creates a new SmartTasker instance and reads that stored progress.

{% endstep %}

{% step %}

### Test a return visit

1. Send `/learn`: expect two buttons and `0/2 done`.
2. Tap **Done: Read the introduction**: expect a saved message and one remaining button.
3. Send `/learn` again: the introduction should still be completed.
4. Send `/learn-done intro` again: expect **already marked done**, with no extra completion.
5. Send `/learn-done unknown`: expect an error and unchanged progress.
6. Complete the second task: expect **All 2 learning steps are done**.
7. Open the bot as another test user: that user's checklist starts separately.

Progress is stored in the user property `SmartTasker.help-checklist-v1:completedTasks`. Its bookkeeping includes counters such as `totalReward`; those values are internal to this example's checklist and are not an actual payment or a ResLib balance. The `balance` of one SmartTasker instance is not an automatically saved wallet.

Keep the task IDs, their order and the checklist name consistent across both commands and return visits. If you redesign the checklist, use a new versioned name and handle any old progress deliberately.

For actual rewards or restricted actions, verify completion using trusted data before changing a balance. Neither a callback string nor typing `/learn-done …` proves that a real-world task was completed.

Next: [SmartTasker reference](../libraries/smart-bot.md#smarttasker-for-task-progress), [inline buttons](../bjs/inline.md), or [a two-language menu](smartbot-menu.md).

{% endstep %}

{% endstepper %}
