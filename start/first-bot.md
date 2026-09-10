---
description: Connect a Telegram bot to Bots.Business and make it reply to /start from the current mobile app, without writing
  code.
---


# Create your first Telegram bot

By the end of this guide, your bot will reply to `/start` in Telegram. You need a Telegram account and a signed-in Bots.Business account. If this is your first visit, [create an account and sign in](sign-in.md) first.

{% stepper %}

{% step %}

### Create the bot in Telegram <a href="#1-create-the-bot-in-telegram" id="1-create-the-bot-in-telegram"></a>

1. Open the official [@BotFather](https://t.me/BotFather) in Telegram and tap **Start** if shown. Send the following messages to BotFather, not to your new bot.
2. Send `/newbot` to begin creating a bot.
3. When asked for a **name**, send a display name, such as `My first bot`. This is the name people see in Telegram.
4. When asked for a **username**, choose a unique one ending in `bot`, such as `alex_practice_2026_bot`. Use 5–32 characters: Latin letters, digits or underscores, with no spaces. If BotFather says it is taken, choose another and send it again.
5. After BotFather confirms creation, copy the complete **bot token** from its reply. The token has digits before a colon (`:`) and a long string after it. Copy only that value, without surrounding text or spaces. The bot's `@username` and `t.me/...` link are not its token.

<figure><img src="../.gitbook/assets/telegram-botfather-create-en.png" alt="English BotFather chat showing /newbot, a display name, a username ending in bot, and the token location with the token hidden" width="430"><figcaption>Creating a bot in Telegram (English). The token is hidden. Choose your own name and username. Screenshot: <a href="https://jozefcipa.com/blog/watching-github-repo-stars-via-telegram/">Jozef Cipa</a>.</figcaption></figure>

**Check:** you now have a bot username and its token. Keep the token ready for the **Token** field in the next step. The new bot will start replying after you connect it and add a command below.

These steps follow Telegram's [bot creation guide](https://core.telegram.org/bots/features#creating-a-new-bot).

{% hint style="info" %}
<img src="../.gitbook/assets/mel-02-help-menu-mobile.webp" alt="" width="64">

**Mel’s tip**

The token belongs to the bot. It is different from your Bots.Business password and account API key. Paste it only into the bot configuration; someone who has it can control the Telegram bot.
{% endhint %}

<details>
<summary>BotFather says the username is already taken</summary>

Choose another username and send it in the same BotFather conversation. For example, add a distinctive prefix or digits before the final `bot`. You do not need to start `/newbot` again.

<figure><img src="../.gitbook/assets/telegram-botfather-username-taken-en.jpg" alt="English BotFather chat rejecting an occupied username and accepting a different one" width="430"><figcaption>When a username is taken, send a different one that still ends in bot. The names shown are examples. Screenshot: <a href="https://www.turtle-techies.com/how-to-create-a-telegram-bot-with-python/">Turtle Techies</a>.</figcaption></figure>

</details>

<details>
<summary>Already have a bot, or cannot find its token?</summary>

If you already created the bot, use the token from its BotFather creation message and continue to the next step. You do not need to send `/newbot` again. A token that was later replaced will no longer work.

If you lost the token or need to replace an exposed one, Telegram documents the [`/token` command](https://core.telegram.org/bots/features#generating-an-authentication-token). Send it to BotFather and follow its prompts for your bot. Put the replacement token into Bots.Business too: **Dashboard → Edit bot → Token** for a bot you already added.

</details>

{% endstep %}

{% step %}

### Add it to Bots.Business <a href="#2-add-it-to-botsbusiness" id="2-add-it-to-botsbusiness"></a>

1. Open **My bots** and choose **Create bot**. If the list already has bots, tap **+**.
2. Enter a useful **Name**, such as `My first bot`.
3. Paste your token into **Token** and tap **CREATE** below the form.
4. Open the new bot to enter its workspace.

A bot can be saved before its token is supplied, but it needs a valid token to connect to Telegram. If you skipped the field, open **Dashboard → Edit bot** and add it.

<figure><img src="../.gitbook/assets/mobile-add-bot.png" alt="The current New bot form with Name, Token, and the CREATE button"><figcaption>The current New bot form with Name, Token, and the CREATE button</figcaption></figure>

{% endstep %}

{% step %}

### Give `/start` an answer <a href="#3-give-start-an-answer" id="3-give-start-an-answer"></a>

1. Open **Commands**.
2. Tap **+** and choose **New command**.
3. Set **Command** to `/start`.
4. Set **Answer** to `Hello! My bot is working.`
5. Leave **Wait for answer** off and the BJS code empty for this first example.
6. Tap **Create** in the command form. For an existing command, open **⋮ → Options**, change its fields, and tap **Edit**. The code editor's **Save** button is used for code changes.

The Answer field is enough for a simple response. You do not need to install a library or write JavaScript for this step.

<figure><img src="../.gitbook/assets/mobile-command-answer.png" alt="Example of the /start Command and Answer fields in Edit metadata"><figcaption>Example of the /start Command and Answer fields in Edit metadata</figcaption></figure>

The screenshot shows demonstration text. For this tutorial, keep the Answer you entered in step 4.

{% endstep %}

{% step %}

### Launch and test <a href="#4-launch-and-test" id="4-launch-and-test"></a>

Open **Dashboard**, tap **Launch bot**, and check its status. Use **Open** to go to the bot in Telegram. Tap Telegram's **Start** button or send `/start` yourself.

You should receive `Hello! My bot is working.` Saving a command alone does not send it to a Telegram chat; sending `/start` triggers it.

<figure><img src="../.gitbook/assets/mobile-dashboard.png" alt="Bot Dashboard with status, Open, Edit bot, and the launch control"><figcaption>Bot Dashboard with status, Open, Edit bot, and the launch control</figcaption></figure>

{% endstep %}

{% endstepper %}

## If the answer does not arrive

- Check that you opened the same bot username as the token you added.
- Check that the bot is running and the command was saved.
- Open **Errors** for the bot. Start with plain Answer text to avoid formatting errors.
- Check that **Wait for answer** is off for this example and that no group restriction is set.
- If another hosting service uses the same Telegram bot, check its connection before moving the bot between services.

Use [Bot does not reply](../troubleshooting/bot-not-responding.md) for the full checklist.

## Next: add a menu

Create a second command and [connect it to a reply keyboard](../app/reply-keyboard.md). To start programming, follow [JavaScript basics for BJS beginners](../bjs/javascript-basics.md). If you already know JavaScript, continue with [BJS execution and APIs](../bjs/README.md).
