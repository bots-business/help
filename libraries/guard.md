---
description: Restrict a command folder to approved internal BB user IDs with the Guard library.
---


# Restrict administrator commands with Guard

`Libs.Guard` checks whether the current user may execute commands in a configured folder. Install `Guard` for the bot, then configure it before relying on its checks.

{% stepper %}

{% step %}

### Configure the administrator list

1. Follow [trusted administrator setup](../bjs/security.md#restrict-a-command-to-a-trusted-telegram-user) to get your own Telegram user ID. Create a temporary setup command with that explicit ID check, followed by `Libs.Guard.setup()`. Leave **Answer** and **Keyboard** empty and **Wait for answer** off. Run it from your own account in a private test conversation.
2. Open the bot's **Admin Panel** in the mobile app and open **Guard**.
3. Set **Admin IDs** to the approved users' internal BB IDs, separated by commas. These are `user.id`, not Telegram IDs.
4. Set **Commands folder** to a folder such as `admins` and move restricted commands into it.
5. Optionally set the unauthorized-access command to `/access-denied`. Keep that response command outside the protected folder to avoid a denial loop.
6. For every protected command, clear **Answer** and **Keyboard**, and turn off **Wait for answer**. Move restricted replies into BJS after the permission check.
7. Save the panel and commands, then remove or independently protect the temporary setup command.

The initial setup records the user who runs it. Letting an arbitrary user run setup first could give them that role.

{% endstep %}

{% step %}

### Apply the check

At the beginning of the **Before all** command `@`:

{% code title="Apply the check · Example 1" overflow="wrap" %}
```javascript
if (!Libs.Guard.verifyAccess()) {
  return;
}
```
{% endcode %}

For `/access-denied`:

{% code title="Apply the check · Example 2" overflow="wrap" %}
```javascript
Bot.sendMessage("This command is available to bot administrators only.");
```
{% endcode %}

A BJS `return`, including one in `@`, does not suppress the selected command's metadata **Answer** or **Keyboard**. Those fields must remain empty on protected commands. Send restricted messages and buttons from BJS only after authorization.

Check both an allowed and a different user in Telegram. An allowed user should reach the protected command; with empty Answer/Keyboard, the other user should only receive the configured denial response, without protected text or buttons.

{% endstep %}

{% endstepper %}

## Check a specific user

{% code title="Check a specific user · Example 3" overflow="wrap" %}
```javascript
const allowed = Libs.Guard.isAdmin(user.id);
Bot.sendMessage(allowed ? "Administrator access." : "Regular user access.");
```
{% endcode %}

`isAdmin(bbUserId)` uses the configured IDs. The library does not automatically equate a Telegram group administrator with a Bots.Business administrator.

## Know the boundaries

`verifyAccess()` allows execution when no Guard panel exists or the current command is outside the configured folder. For every sensitive administrator command, also place this direct check at its beginning:

{% code title="Know the boundaries · Example 4" overflow="wrap" %}
```javascript
if (!user || !Libs.Guard.isAdmin(user.id)) {
  return;
}
// The protected action follows this check.
```
{% endcode %}

This direct check refuses access when the panel or administrator list is missing, and does not depend on the command's folder. Keep it on actions that transfer money or change protected settings; the global folder check alone is insufficient if the configuration disappears or a command is moved.

The check expects a current user and command context. Background jobs and provider webhooks need their own trusted-entry rules. Hiding a keyboard button does not protect the command behind it.

For panel field contracts, see [AdminPanel BJS](../bjs/admin-panel.md). For authenticated external requests, see [Webhooks](webhooks.md).
