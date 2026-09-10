---
description: Diagnose BJS errors by command and context, fix common callback and method mistakes, and customize the private-chat
  error response with the ! command.
---


# BJS errors and debugging

Start with the failed command and its error message. A syntax error, missing context, unsupported method, failed external request, and exhausted execution limit require different fixes.

## A useful debugging sequence

1. Reproduce the command on a test bot with a specific message or button action.
2. Open the bot's Errors screen and inspect the latest relevant entry.
3. Check the named command and code location, then identify its trigger and required variables.
4. Reduce the example to one action. Add the next step only after the first works.
5. Repeat the original flow, including callbacks and a later read when data persistence matters.

Use the app guide for [finding an error and its command](../troubleshooting/errors.md).

## Inspect only the values you need

{% code title="Inspect only the values you need · Example 1" overflow="wrap" %}
```javascript
// Temporary diagnostic command on a test bot.
Bot.inspect({
  has_user: !!user,
  has_chat: !!chat,
  parameters: String(params || "").slice(0, 100),
  option_keys: options ? Object.keys(options) : []
});
```
{% endcode %}

Do not print the entire `bot`, `owner`, request headers, account response, or credential-bearing panel. Remove temporary diagnostics when the issue is understood.

## Common problems

| Symptom | What to check |
| --- | --- |
| Method is not defined | Exact object/method spelling: `HTTP`, `Api`, `AdminPanel.getFieldValue`; a method copied from ordinary JavaScript may not exist in BJS |
| `User.getProp` reports user is not defined (the error may name `User.getProperty`) | The trigger has no current user; check [context](context.md) and explicit scope |
| Cannot read a property of undefined/null | Guard the parent object; a manually invoked callback has no remote result |
| HTTP JSON parse error | Check status and body format before `JSON.parse(content)` |
| API callback does not run | Saved callback command name, `on_result` vs HTTP `success`, backend method support, and error callback |
| “Too many sub commands” | Recursive or overly long command/callback chains; split work into bounded steps |
| Timeout | Unbounded loops, repeated requests, expensive data loading, or execution limits |
| Property changed immediately but not in the next command | Verify write scope, persisted type, later read, and [false-like default behavior](user-properties.md#default-values-and-zerofalse) |
| New defaults did not replace panel values | Admin Panel preserves old values unless intentionally forced |

A JavaScript `try/catch` handles errors thrown while that BJS code runs. It does not turn a queued remote request into a synchronous operation. Use [HTTP success/error commands](http.md) or [Api result/error commands](telegram-api.md) for remote outcomes.

<details>
<summary>Customize the error command: !</summary>

## Customize the error command: !

Create a command named exactly `!` to customize the ordinary private-chat error response:

{% code title="!" overflow="wrap" %}
```javascript
// Command: !
Bot.sendMessage("Something went wrong. Please try again or use /help.");
```
{% endcode %}

Keep it short and dependable. Do not put the original risky request inside this command. The backend avoids recursively showing another error response when `!` itself fails, so a broken error command may produce no useful chat message.

This response is for the private-chat error-notification flow. It is not a global replacement for every API callback, web response, group failure, or background-task error. Continue checking the error log even when a friendly message is shown.

</details>

## Make examples easier to maintain

Use named variables, small functions, clear guards, and one purpose per command. Avoid copying the same complex validation into several slightly different snippets; use a shared helper only when its execution context and return behavior are understood. Keep callback names and their handlers together in the article or project.

For a bot that never reaches the command at all, use [Bot not responding](../troubleshooting/bot-not-responding.md). For an execution problem, start with [BJS context](context.md) and the relevant method reference.
