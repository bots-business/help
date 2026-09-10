---
description: Build a Telegram Mini App form on an external HTTPS host, open it with a reply keyboard, and receive validated
  form data in a Bots.Business BJS command.
---


# Build a Telegram Mini App form

This example opens a small form inside Telegram. The user chooses a help topic, submits it, and receives a normal bot reply. You need a [working bot](../start/first-bot.md), permission to edit its commands, and an external host that serves your HTML file over HTTPS.

**Host the HTML outside Bots.Business.** The current [BJS Web App renderer](../bjs/web-app.md) serves JSON; HTML responses are disabled and return 404. `WebApp.render` cannot host this form.

{% stepper %}

{% step %}

### Publish the form <a href="#1-publish-the-form" id="1-publish-the-form"></a>

Save the following as `form.html` on your HTTPS host. Replace the example URL in the next step with its public address. The page contains no bot token and needs no separate application backend.

{% code title="1. Publish the form · Example 1" overflow="wrap" %}
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Choose a help topic</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0; padding: 24px; font: 17px/1.5 system-ui, sans-serif;
      background: var(--tg-theme-bg-color, #fff);
      color: var(--tg-theme-text-color, #17212b);
    }
    main { max-width: 28rem; margin: auto; }
    h1 { font-size: 1.5rem; }
    label { display: block; margin-bottom: 8px; }
    select, button {
      width: 100%; min-height: 48px; padding: 10px;
      font: inherit; border: 1px solid #88939d; border-radius: 8px;
    }
    select {
      background: var(--tg-theme-bg-color, #fff);
      color: var(--tg-theme-text-color, #17212b);
    }
    button {
      margin-top: 16px; border: 0;
      background: var(--tg-theme-button-color, #1769aa);
      color: var(--tg-theme-button-text-color, #fff);
    }
  </style>
</head>
<body>
  <main>
    <h1>Choose a help topic</h1>
    <form id="topic-form">
      <label for="topic">What would you like to learn?</label>
      <select id="topic" required>
        <option value="">Choose a topic</option>
        <option value="commands">Commands</option>
        <option value="properties">Properties</option>
      </select>
      <button type="submit">Send to bot</button>
      <p id="status" role="status"></p>
    </form>
  </main>
  <script>
    const tg = window.Telegram && window.Telegram.WebApp;
    if (tg) tg.ready();
    document.getElementById("topic-form").addEventListener("submit", event => {
      event.preventDefault();
      const topic = document.getElementById("topic").value;
      if (topic !== "commands" && topic !== "properties") return;
      try {
        if (!tg) throw new Error("SDK unavailable");
        tg.sendData(JSON.stringify({ type: "help-topic", version: 1, topic }));
      } catch (error) {
        document.getElementById("status").textContent =
          "Open this form using the bot's Open form keyboard button.";
      }
    });
  </script>
</body>
</html>
```
{% endcode %}

The official SDK connects the page to Telegram. `sendData` sends a string to the bot and closes the Mini App; its payload limit is 4,096 bytes. This small object fits comfortably. [Telegram's keyboard-button Mini Apps](https://core.telegram.org/bots/webapps#keyboard-button-mini-apps).

{% endstep %}

{% step %}

### Create `/open-form` <a href="#2-create-open-form" id="2-create-open-form"></a>

In **Commands**, create `/open-form`, leave **Answer** and **Keyboard** empty, and leave **Wait for answer** off. Paste this BJS into the code editor and tap **Save**:

{% code title="2. Create /open-form · Example 2" overflow="wrap" %}
```javascript
Api.sendMessage({
  text: "Choose a topic in the form below.",
  reply_markup: {
    keyboard: [[{
      text: "Open form",
      web_app: { url: "https://YOUR-HOST.example/form.html" }
    }]],
    resize_keyboard: true
  }
});
```
{% endcode %}

Replace the entire URL with your working HTTPS address. `Api.sendMessage` uses the current Telegram chat when `chat_id` is omitted.

Use the **reply keyboard below the message input**, in a private chat with the bot. This `web_app` button is private-chat only. An inline button, menu button, or ordinary browser tab does not use this example's `sendData` return path. [Telegram KeyboardButton](https://core.telegram.org/bots/api#keyboardbutton).

{% endstep %}

{% step %}

### Receive the selection in `*` <a href="#3-receive-the-selection-in" id="3-receive-the-selection-in"></a>

Create a command whose name is exactly `*`. Leave **Answer** and **Keyboard** empty and **Wait for answer** off, then save this BJS:

{% code title="3. Receive the selection in * · Example 3" overflow="wrap" %}
```javascript
if (!request || !request.web_app_data) { return; }

const topic = readTopic(request.web_app_data.data);
if (!topic) {
  Bot.sendMessage("Please open the form and choose a listed topic.");
  return;
}
const topicName = topic === "commands" ? "Commands" : "Properties";
Bot.sendMessage("You selected: " + topicName + ".");

function readTopic(raw) {
  // This example's schema needs far less than 512 characters.
  if (typeof raw !== "string" || raw.length > 512) { return null; }
  let data;
  try { data = JSON.parse(raw); }
  catch (error) { return null; }
  if (!data || Array.isArray(data) ||
      data.type !== "help-topic" || data.version !== 1 ||
      (data.topic !== "commands" && data.topic !== "properties")) {
    return null;
  }
  return data.topic;
}
```
{% endcode %}

Telegram delivers the submission as a service message. In BJS, its data is `request.web_app_data.data`; the `message` text may be empty. The `*` command handles that update.

If your bot already has `*`, integrate this handling into it and preserve its other branches, such as [contact, location and media handling](telegram-updates.md). Keep `readTopic` in the same command as its caller. The first line above intentionally ignores other updates; replacing an existing wildcard with this entire example would remove its previous behavior.

Validate in BJS even though the form has a fixed dropdown. A modified client can send arbitrary `data` and `button_text`. This example accepts two topic identifiers and constructs its reply from fixed labels. Do not use submitted fields as evidence of payment, identity, or permission. [Telegram WebAppData](https://core.telegram.org/bots/api#webappdata).

{% endstep %}

{% step %}

### Test the complete flow <a href="#4-test-the-complete-flow" id="4-test-the-complete-flow"></a>

Launch the bot, send `/open-form` in your private Telegram chat, tap **Open form**, choose **Commands**, and tap **Send to bot**. The form should close and the bot should reply `You selected: Commands.` Repeat with **Properties**.

For this test, use a bot that is not waiting for an answer to another command. An active **Wait for answer** takes the next update before normal wildcard dispatch; turn it off for these example commands and finish or cancel any previous waiting flow. See [command behavior](../app/commands.md).

If the form opens but no reply arrives, check the launch button type, saved `*` code, waiting state, bot status, and [Errors](../troubleshooting/errors.md). If the page does not open, check its HTTPS address and SDK loading.

This form returns data through Telegram and does not authenticate requests to an external backend. If you later add such a backend, implement Telegram's signed `initData` validation there before relying on user identity; `initDataUnsafe` alone is insufficient. [Telegram validation guide](https://core.telegram.org/bots/webapps#validating-data-received-via-the-mini-app).

{% endstep %}

{% endstepper %}
