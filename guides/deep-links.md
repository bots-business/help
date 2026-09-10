---
description: Pass a short start parameter into your Telegram bot, read it in BJS, and validate it before using it as a command
  or referral value.
---


# Pass a value when a user starts the bot

Use a Telegram start link when a user should arrive with a short value, such as a campaign or product code:

{% code title="Example · Example 1" overflow="wrap" %}
```text
https://t.me/YOUR_BOT_USERNAME?start=summer_2026
```
{% endcode %}

Replace `YOUR_BOT_USERNAME` with your bot's username. The `/start` command receives the supplied value as BJS `params`.

## Read a known campaign value

Create `/start`, leave **Wait for answer** off, and use this BJS:

{% code title="Read a known campaign value · Example 2" overflow="wrap" %}
```javascript
if (params === "summer_2026") {
  Bot.sendMessage("Welcome to our summer campaign. Send /help to continue.");
} else {
  Bot.sendMessage("Welcome! Send /help to continue.");
}
```
{% endcode %}

Create `/help` with your next steps. Test a plain `/start`, the campaign link, and a link containing an unknown value. The unknown value should take the normal welcome path.

## Parameter rules

Telegram start parameters support letters, digits, `_`, and `-`, with a maximum of 64 characters. See Telegram's [deep-linking specification](https://core.telegram.org/bots/features#deep-linking).

Treat the value as user-controlled input. It can be edited or reused; receiving a value is not proof of identity, payment, or permission. Keep credentials out of start links and chat responses. Use an explicit allowlist of supported values when the link selects an action.

For referral handling and attribution, use the [referral library guide](../libraries/referrals.md) instead of granting a reward merely because a parameter was present.

## If params is empty

Check that the link uses `?start=`, that the user completed Telegram's start action, and that you are reading the value in the `/start` execution. A later command does not automatically inherit the original link parameter; store needed state deliberately.
