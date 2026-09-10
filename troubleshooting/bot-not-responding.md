---
description: "Diagnose a Telegram bot that does not reply: check token, launch state, commands, pending input, groups, errors, and iterations."
---

# My bot does not reply

Start with a plain `/start` Answer in a private Telegram chat. This separates the connection and command setup from more complex BJS behavior.

## 1. Confirm the bot and its status

Open **Dashboard** in Bots.Business. Check the name, configured token, and status. Tap **Launch bot** if it is stopped, then wait for the result. Use **Open** to reach the corresponding Telegram bot.

If the token was regenerated in BotFather, update it through **Edit bot**. Check whether another service is using the same Telegram bot connection.

## 2. Check the saved command

Open **Commands** and inspect `/start`. Clear search and folder filters if necessary. For this test, set an ordinary text Answer, leave code empty, turn off **Wait for answer**, and remove any group restriction. Save, then send `/start` again.

Main command names match exactly. A command named `/START` is not a dependable substitute for `/start`; use consistent lowercase names. For buttons, check the [label and alias](../app/reply-keyboard.md).

## 3. Check whether the bot is waiting

If you turned on **Wait for answer**, the first command sends its prompt and the BJS runs after a reply. That delay can be intentional. A known command or alias cancels the pending wait and starts the matching command instead.

See [collecting input](../app/collect-input.md) to test the complete conversation, including cancellation.

## 4. Read the actual error

Open **Errors** and reproduce the problem once. Inspect the newest relevant entry. **Go to command** can take you to its code and reported line.

Formatting errors, unknown methods, missing callback commands, and absent user context require different fixes. Follow [Errors and debugging](errors.md) rather than repeatedly relaunching the bot.

## 5. Check access and resources

- In **Chats**, check whether this chat is blocked.
- A command's **Allowed only for group** restriction requires membership in a Bots.Business user group.
- Open **Iterations** and check the current allowance and Extra Points.
- If a private chat works but a Telegram group does not, check the bot's group permissions and Telegram [privacy mode](https://core.telegram.org/bots/features#privacy-mode).
- If a simple answer works but a long command times out, investigate [performance](performance.md).

## Still stuck?

Record the bot ID, command name, approximate time, exact error, and whether the failure happens in private chats, groups, or background work. Include a small example that reproduces it. Use **Settings → Support chat** and omit tokens, passwords, API keys, and private conversation contents.
