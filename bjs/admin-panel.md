---
description: Create a Bots.Business Admin Panel with BJS, preserve saved values, read fields with getFieldValue or getPanelValues,
  and run a command after saving.
---


# Create an Admin Panel with BJS

An Admin Panel is a form the bot owner can open in the app. Define its fields with BJS, then read the saved settings in your commands. If you only need to fill an existing form, start with [using an Admin Panel](../app/admin-panel.md).

## Define new Admin Panel

Create a setup command `/setup-panel` on a bot you control. Run it once from a controlled owner/test context; do not expose configuration-reset commands to every user.

{% code title="Define new Admin Panel · Example 1" overflow="wrap" %}
```javascript
AdminPanel.setPanel({
  panel_name: "welcome",
  data: {
    title: "Welcome settings",
    description: "Text used by the welcome command.",
    index: 0,
    button_title: "Save",
    fields: [
      {
        name: "welcome_text",
        title: "Welcome message",
        type: "string",
        placeholder: "Enter a short greeting",
        value: "Welcome!"
      },
      {
        name: "show_help",
        title: "Show the help command",
        type: "checkbox",
        value: true
      }
    ]
  }
});
Bot.sendMessage("Open Admin Panel in the app to edit the welcome settings.");
```
{% endcode %}

Open the bot's Admin Panel in the app, change the text, and save. Then use a separate `/welcome` command to read it.

{% tabs %}

{% tab title="Read one field" %}

## Getting field value from Panel

{% code title="/welcome" overflow="wrap" %}
```javascript
// Command: /welcome
var greeting = AdminPanel.getFieldValue({
  panel_name: "welcome",
  field_name: "welcome_text"
});
Bot.sendMessage(greeting || "Welcome!", { parse_mode: null });
```
{% endcode %}

The method name is **`getFieldValue`**. There is no `AdminPanel.getPanelValue` method. A missing panel or field produces no value, so provide the appropriate fallback for your command.

{% endtab %}

{% tab title="Read all fields" %}

## Getting all fields values from Panel

{% code title="/welcome-with-help" overflow="wrap" %}
```javascript
// Command: /welcome-with-help
var values = AdminPanel.getPanelValues("welcome");
Bot.sendMessage(values.welcome_text || "Welcome!", { parse_mode: null });
if (values.show_help === true) {
  Bot.sendMessage("Send /help for available commands.");
}
```
{% endcode %}

`getPanelValues` returns a name-to-value object, for example `{welcome_text: "Welcome!", show_help: true}`. For a missing panel it returns `{}`.

{% endtab %}

{% endtabs %}

## Panel and field options

| Option | Meaning |
| --- | --- |
| `panel_name` | Stable internal panel name used by BJS reads |
| `data.title`, `data.description` | User-facing title and explanation |
| `data.index` | Ordering hint for the panel |
| `data.button_title` | Save/action button label |
| `data.fields` | Array of field definitions |
| Field `name` | Stable key used by `getFieldValue` |
| Field `title`, `description`, `placeholder` | Explain what the owner should enter |
| Field `value` | Initial value, or replacement value when forced |
| Field `type` | `string`, `text`, `integer`, `float`, `password`, or `checkbox` |
| Field `hidden` | Hide the field from the form; not an access-control rule |

A checkbox is an on/off control. Integer/float fields accept numeric input; a password field masks its displayed value. Masking does not make it safe to publish the value in bot messages or share access to the whole bot.

## Force

`AdminPanel.setPanel` preserves existing values for fields with the same name unless you pass `force: true`. This lets you change labels or add a field without resetting the owner's settings.

Use `force: true` only for an intentional reset. Renaming a field creates a different key, so migrate its saved value deliberately. Saving a single field is usually safer than replacing the whole panel.

## Setting field value to Panel

{% code title="Setting field value to Panel · Example 4" overflow="wrap" %}
```javascript
var changed = AdminPanel.setFieldValue({
  panel_name: "welcome",
  field_name: "welcome_text",
  value: "Hello again!"
});
Bot.sendMessage(changed ? "Updated." : "Create the panel and field first.");
```
{% endcode %}

An existing field returns `true` when its update action is created. This method does not create a missing panel or a missing field. Read the value in a later command when checking durable storage.

`AdminPanel.getPanel("welcome")` returns the full panel definition. `AdminPanel.getPanelField({panel_name: "welcome", field_name: "welcome_text"})` returns a field definition. Do not expose these whole objects when a panel contains secrets.

## Run a command on saving

Add `on_saving` inside `data` when defining the panel:

{% code title="Run a command on saving · Example 5" overflow="wrap" %}
```javascript
on_saving: {
  command: "/refresh-welcome"
}
```
{% endcode %}

Create `/refresh-welcome`:

{% code title="/refresh-welcome" overflow="wrap" %}
```javascript
Bot.clearCache("/welcome");
```
{% endcode %}

This fragment belongs in the full `data` object above. The save action runs the named command after storing the panel. If the command needs a user, provide that user's **internal** `user_id` in `on_saving`. Do not assume an app save has the same Telegram context as a chat message.

## Troubleshooting

If the form is missing, verify the setup command ran successfully and the correct bot is open. If a new default did not appear, existing values were probably preserved intentionally. If a read is undefined, compare `panel_name` and field `name`, including case. If save succeeds but follow-up work fails, check the callback command and its required context in [BJS errors](errors.md).

Next: [properties](user-properties.md), [caching](caching.md), or the distinct [BBAdmin API](bb-admin.md).
