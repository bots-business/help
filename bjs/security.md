---
description: Protect BJS commands and credentials, validate user input, understand sandbox limits, and avoid trusting buttons, web parameters, or unsafe cached actions.
---

# BJS security and safe command design

Every command that changes privileged data needs an explicit permission check. A hidden command, inline button, obscure name, or generated web URL is not an authorization rule.

## Restrict a command to a trusted Telegram user

Configure the permitted Telegram user ID yourself in the mobile command editor. Linking an account does not guarantee that `owner.telegramid` is populated, so do not use it to authorize a linked owner.

1. Create a temporary `/my-id` command with empty **Answer** and **Keyboard**, and this BJS:

```javascript
if (!user || !user.telegramid) { return; }
Bot.sendMessage(String(user.telegramid));
```

2. Send `/my-id` from your own Telegram account in a private conversation with the bot. Copy the returned ID. This command only reports the caller's ID; it does not grant access.
3. Create `/owner-status` below. In the mobile editor, replace `YOUR_TELEGRAM_USER_ID` with the ID you just obtained, keeping the quotes. Use a Telegram user ID, not an internal BB ID, chat ID, username, or value supplied by another user. The unchanged placeholder denies access to everyone.
4. Leave **Answer** and **Keyboard** empty, turn off **Wait for answer**, and save the code. Keep privileged replies inside BJS after the check: `return` stops BJS, but does not suppress metadata Answer/Keyboard.
5. Test with your account and a different Telegram account. Only your configured account should receive the confirmation. Remove `/my-id` when finished.

The protected command:

```javascript
// Command: /owner-status
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || !user.telegramid ||
    String(user.telegramid) !== ADMIN_TELEGRAM_ID) {
  return;
}
Bot.sendMessage("Administrator access confirmed.");
```

Keep the guard inside each privileged execution path, including commands reachable through buttons. Set the same trusted ID explicitly in each example that uses `ADMIN_TELEGRAM_ID`; never initialize it from the first visitor or chat input. For several administrators, configure [Guard](../libraries/guard.md). If you implement a separate role system, store roles in trusted bot data and validate changes to that role data too. Do not let a user choose the `user_id` that decides their own permissions.

This guard is for Telegram-triggered commands. A web endpoint or app-triggered command may lack `user`; design an appropriate trusted flow for that trigger rather than removing the check until it passes.

## Validate input before using it

```javascript
// Command: /quantity
var raw = String(params || "").trim();
if (!/^\d+$/.test(raw)) {
  Bot.sendMessage("Enter a whole quantity from 1 to 10.");
  return;
}
var quantity = Number(raw);
if (quantity < 1 || quantity > 10) {
  Bot.sendMessage("Enter a whole quantity from 1 to 10.");
  return;
}
Bot.sendMessage("Selected quantity: " + quantity);
```

Validate the full input, not just a numeric prefix. Limit text length, confirm expected options, and reject unknown actions. For untrusted text in ordinary replies, `parse_mode: null` avoids treating it as markup. Escape data for the particular HTML, URL, or JSON context when rendering it elsewhere.

## Keep credentials out of replies

Treat bot tokens, service API keys, password-reset information, and webhook secrets as credentials. Do not publish them in examples, screenshots, source exports, query strings, or `Bot.inspect` output. Masking an Admin Panel password field affects its display, not every other route by which your code can reveal it.

The current BJS HTTP transport does not validate HTTPS server certificates. Do not rely on an HTTPS URL alone for confidential or authenticated communication with an external service. Review the [HTTP transport limitation](http.md#https-transport-limitation) before sending secrets or trusting responses for sensitive actions.

Keep HTTP destinations under your control. Do not accept arbitrary URLs, headers, or callback command names from chat input and then execute them with privileged credentials.

## Understand the sandbox

BJS has its own available globals and limits. Do not use Node.js `require`, filesystem access, browser DOM APIs, Promises, `queueMicrotask`, the `Function` constructor, Proxy construction, WebAssembly, or top-level `await` as if this were a browser or Node process. Use supported [BJS callbacks](README.md) and [scheduled commands](background.md).

Although ordinary expression evaluation can exist in the runtime, never use `eval` on user-supplied input. Templates also evaluate expressions; only trusted template authors should control the code between `<% ... %>` tags.

## Side effects and repeated execution

A user can send the same command or click a button again. Remote services can retry events. Use a verified event identifier and an appropriate idempotency design for important actions; a simple property read followed by a write is not an atomic transaction across concurrent executions.

Do not [cache](caching.md) commands that grant access, increment balances, or perform a one-time operation. Cache replay and retries must not repeat an important side effect accidentally.

For [Web App](web-app.md) requests, a generated URL and user-supplied identifiers do not authenticate the user. Validate signed provider data on a trusted server before making privileged changes. A payment callback requires verification of the payment provider's event and amount, not just a successful-looking JSON body.

## Troubleshooting

If an administrator command does not respond, check the saved `ADMIN_TELEGRAM_ID`, its quotes, and which Telegram account sent the command. Linking accounts alone does not populate this setting. Test a different account too, and check that protected text is absent from metadata Answer/Keyboard. If a sandbox feature is unavailable, use its supported BJS alternative. For unexpected repeats, trace the triggering command, callbacks, queued task, and cache separately through [BJS errors](errors.md).
