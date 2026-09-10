---
description: Ask a question with Wait for answer, process the reply in BJS, save user data, and provide a cancel path.
---


# Ask a question and collect the reply

{% hint style="info" %}
<img src="../.gitbook/assets/mel-02-help-menu-mobile.webp" alt="" width="64">

**Mel’s tip**

Turn on **Wait for answer** when a command should send a prompt and use the next reply as input. Its BJS runs after the reply, rather than when the prompt is first sent.
{% endhint %}

{% stepper %}

{% step %}

### Ask for a name

Create a command `/name` with these settings:

| Setting | Value |
| --- | --- |
| Answer | `What should I call you? Send /cancel to stop.` |
| Wait for answer | On |
| Auto retry time in seconds | Empty |

Put this in the BJS editor and save:

{% code title="Ask for a name · Example 1" overflow="wrap" %}
```javascript
if (!message || !message.trim()) {
  Bot.sendMessage("Please send /name again and reply with text.");
  return;
}

var displayName = message.trim();
User.setProp("display_name", displayName);
Bot.sendMessage({
  text: "Thanks, " + displayName + ". Your name is saved.",
  parse_mode: null
});
```
{% endcode %}

The explicit parse mode keeps user input from being interpreted as Markdown. The property belongs to the current bot user; see [properties and scopes](../bjs/user-properties.md).

{% endstep %}

{% step %}

### Add a cancel command

Create a separate `/cancel` command with Answer `Cancelled. Send /name when you are ready.` Leave **Wait for answer** off for `/cancel`.

When a pending reply matches an existing command or alias, Bots.Business cancels the pending wait and processes the matching command. This is why `/cancel` must actually exist. A string check inside `/name` is not the only way to cancel.

This also means that a reply such as `Help` can leave the input flow if `Help` is a command alias. Choose prompts and button labels with that behavior in mind.

{% endstep %}

{% step %}

### Test the whole conversation

1. Send `/name`; expect the question and no saved-name message yet.
2. Reply `Alex`; expect the confirmation.
3. Open that user's [Properties](properties.md) and check `display_name`.
4. Send `/name` again, then `/cancel`; expect cancellation.
5. Send ordinary text after cancellation; it should no longer be treated as the answer to `/name`.

{% endstep %}

{% endstepper %}

## Validation and repeated questions

The pending request is consumed after handling the reply, including when its BJS fails. An error does not automatically keep the same question active. The example above explicitly asks the user to start `/name` again after invalid input.

For a multi-step form, store each accepted value and deliberately start the next question. Keep a cancel command available, and avoid command names or aliases that collide with likely answers.

If no prompt appears, check that the command has an Answer and the bot is running. If the prompt appears but the reply fails, open [Errors](../troubleshooting/errors.md) and confirm that the code uses the reply's `message` and a valid user context.

For numeric input with validation, repeated prompts, and cancellation, follow the complete [quantity dialog](../guides/quantity-dialog.md).
