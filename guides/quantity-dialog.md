---
description: Ask for a quantity, validate it with SmartAmountDialog, repeat invalid input and support cancellation in a complete
  BJS conversation.
---


# Ask for a quantity and validate the reply

This private-chat exercise asks for a whole-number quantity from 1 to 10 and saves it for the current user. It demonstrates input validation without placing an order or transferring anything.

`SmartAmountDialog` is included in the runtime. You do not need to install a Store library.

{% stepper %}

{% step %}

### Create the question

Create `/quantity` with:

| Field | Value |
| --- | --- |
| Answer | `How many items? Enter a whole number from 1 to 10, or send /cancel.` |
| Wait for answer | On |
| Keyboard | Empty |
| Allowed only for group | Empty |

Test this conversation in a private chat with your bot. The guard below prevents saving a group reply, but an Answer in command metadata can still send its initial prompt in a group.

Use this BJS for the reply:

{% code title="/quantity" overflow="wrap" %}
```javascript
// Command: /quantity
if (!user || !chat || chat.chat_type !== "private") { return; }
var dialog = new SmartAmountDialog({
  min: 1,
  max: 10,
  curValue: 10,
  onlyInteger: true,
  dialogErrors: {
    invalid: "Enter a number using digits, for example 3.",
    notInteger: "Enter a whole number, for example 3.",
    zero: "No items are available.",
    notEnough: "Choose no more than 10 items.",
    small: "Choose at least 1 item.",
    big: "Choose no more than 10 items."
  }
});
var result = dialog.accept(String(message || "").trim());
if (result !== true) {
  Api.sendMessage({ text: String(result) });
  Bot.runCommand("/quantity");
  return;
}
User.setProp("demo_quantity", dialog.amount);
Api.sendMessage({ text: "Saved quantity: " + dialog.amount });
```
{% endcode %}

{% hint style="info" %}
Without a SmartBot instance, `accept` returns `true` on success or the configured error text. Compare with `true` explicitly: a nonempty error string is also truthy in JavaScript.
{% endhint %}

The first call sends the question. The next reply runs the BJS. After invalid input, `Bot.runCommand("/quantity")` sends the question again and opens another wait. Valid input saves a number and finishes the wait.

{% endstep %}

{% step %}

### Add cancellation

Create the actual command `/cancel` with empty Answer and Keyboard and **Wait for answer** off:

{% code title="/cancel" overflow="wrap" %}
```javascript
// Command: /cancel
Api.sendMessage({ text: "Cancelled. Send /quantity when you are ready." });
```
{% endcode %}

Recognized commands can interrupt a pending question. Defining `/cancel` gives the user an explicit way out; merely mentioning an undefined command in the prompt does not create cancellation behavior.

{% endstep %}

{% step %}

### Check the conversation

| Your message | Expected result |
| --- | --- |
| `/quantity` | The bot asks for a quantity. |
| `hello` | A number error, then the question again. |
| `2.5` | A whole-number error, then the question again. |
| `0` or `11` | A range error, then the question again. |
| `3` | `Saved quantity: 3`; the wait finishes. |
| `/quantity`, then `/cancel` | Cancellation; a later ordinary message is not saved as a quantity. |

Check `demo_quantity` in the user's [Properties](../app/properties.md). Invalid input and cancellation leave the previously accepted value unchanged.

The parser expects digits and, when fractions are enabled, a decimal point. It does not accept a decimal comma, currency symbol, negative sign or scientific notation. This recipe deliberately uses whole numbers.

For a real order, validation is only one step: reread current availability and perform the actual operation through its own checked workflow. `curValue: 10` here is demonstration availability, not a reservation.

See [SmartAmountDialog options](../libraries/smart-bot.md#validate-an-amount) and [how Wait for answer works](../app/collect-input.md).

{% endstep %}

{% endstepper %}
