---
description: Build a Telegram bot that replies to /start and offers a working two-button
  menu, with checks at each stage.
cover: ../.gitbook/assets/cover-first-bot.webp
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

# First bot and menu

**Your result:** a bot that replies to `/start` and a reply keyboard whose two buttons open the matching commands.

**Before you start:** a Telegram account, access to Bots.Business on your phone, and a bot you can use for learning. No JavaScript is needed for this route.

{% stepper %}
{% step %}
### Sign in to the right account

**Prepare:** decide which Bots.Business account will own this bot. If you are already signed in, check **Settings → Profile**.

**Do:** follow [Create an account and sign in](../start/sign-in.md) if needed.

**Check:** you can open My bots under the intended account. If expected bots are missing, check the account before creating another copy.

**Next:** connect one Telegram bot.
{% endstep %}
{% step %}
### Get your first reply

**Prepare:** keep the intended bot's BotFather token available privately.

**Do:** follow all four steps in [Create your first Telegram bot](../start/first-bot.md): create the bot, add its token, give `/start` an Answer, and launch it.

**Check:** sending `/start` in the bot's private Telegram chat returns `Hello! My bot is working.` If it does not, finish [the connection checks](../troubleshooting/bot-not-responding.md) before continuing.

**Next:** create the two commands that your menu will open.
{% endstep %}
{% step %}
### Create the menu's commands

**Prepare:** open the same bot's Commands tab. Keep the `/start` command that already works.

**Do:** use [Create a command on your phone](../app/commands.md#create-a-command-on-your-phone) to create the `/help` and `/about` commands with the `Help` and `About` aliases described in [Build a two-button menu](../app/reply-keyboard.md#build-a-two-button-menu). Give both commands their own Answer. Keep Wait for answer off.

**Check:** send `Help` and `About` as ordinary messages in Telegram. Each must produce its own reply before you connect the buttons. Check capitalization and [aliases](../app/commands.md#names-aliases-and-parameters) if one does not match.

**Next:** make those messages available as buttons.
{% endstep %}
{% step %}
### Add and test the reply keyboard

**Prepare:** both commands reply when typed.

**Do:** finish [Build a two-button menu](../app/reply-keyboard.md#build-a-two-button-menu), then optionally try [separate rows](../app/reply-keyboard.md#put-buttons-on-separate-rows), adding a matching `Contact` command or alias before testing the third button. The later changing-balance example is optional and requires BJS; it is outside this first route.

**Check:** send `/start`, tap both buttons, and confirm that each reply matches the command you tested. A reply keyboard button sends its label as a message.

**Next:** [remember a name and validate input](data-and-dialogs.md).
{% endstep %}
{% endstepper %}

{% hint style="info" %}
<img src="../.gitbook/assets/mel-02-help-menu-mobile.webp" alt="" width="64">

**Mel’s tip**

A Telegram bot token connects your bot. Your Bots.Business account owns its settings and commands. When checking a missing reply, first make sure you opened the bot that matches the configured token.
{% endhint %}

## Keep this checkpoint

You are ready for the next route when `/start`, typed command names, and both buttons work. For an individual feature instead, browse [recipes](../guides/recipes.md).
