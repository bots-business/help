# Keyboard

Fill the keyboard field for command. Keyboard has buttons in the rows. Each button is separated by a comma. Use `\n` for a new row.

![Keyboard in bot](<../.gitbook/assets/image (52).png>)

Example:

```
register, about, \n contacts
```



**The text of the button will be placed in the chat after touching this button.** Use commands aliase on button. Quite pleasant "contact" on the button, than "/contacts"

Emoji also possible. Then the alias should also be with Emoji.

![ Keyboard can be modified on command editing ](<../.gitbook/assets/image (24).png>)

{% hint style="info" %}
The keyboard does not disappear after the user's answers. Bot can send a new keyboard on a new command.
{% endhint %}
