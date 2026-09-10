---
description: Understand BBAdmin account and bot-installation methods, their access restrictions, and how they differ from editable Admin Panels.
---

# BBAdmin account operations

`BBAdmin` performs account-level operations. It is different from `AdminPanel`, which defines settings forms for a bot. Use [Admin Panel](admin-panel.md) when you want fields the owner can edit.

Most BBAdmin methods are restricted to bots owned by a Bots.Business platform administrator. Owning an ordinary bot does not grant that role. A method name existing in BJS does not mean every account may execute it.

## Methods for ordinary bot owners

The backend permits the attraction and bot-installation actions for ordinary owners, with additional checks:

| Method | Purpose |
| --- | --- |
| `BBAdmin.attractUser({email})` | Start the account-attraction flow for an email address |
| `BBAdmin.installBot(options)` | Clone/install an accessible source bot for the intended account |
| `BBAdmin.cloneBot(options)` | Alias of the same installation action |

Installation options include `bot_id`, `email`, `token`, `bot_properties`, `as_protected`, and `run_now`. The backend decides whether immediate execution is allowed; do not assume a requested `run_now` overrides that decision. The source must be accessible to the owner, and installation from a protected bot is refused. Anti-abuse checks also apply.

## Controlled installation example

This command starts a real account operation. Run it only for an intended recipient and a source bot you are allowed to distribute. Replace the sample email with that recipient before using it. Follow [trusted administrator setup](security.md#restrict-a-command-to-a-trusted-telegram-user) and replace `YOUR_TELEGRAM_USER_ID` with your own Telegram user ID in the mobile editor. Leave **Answer** and **Keyboard** empty, and **Wait for answer** off; the guard controls BJS only.

```javascript
// Command: /install-demo
const ADMIN_TELEGRAM_ID = "YOUR_TELEGRAM_USER_ID";
if (!user || !user.telegramid ||
    String(user.telegramid) !== ADMIN_TELEGRAM_ID) {
  return;
}
BBAdmin.installBot({
  bot_id: bot.id,
  email: "recipient@example.com",
  as_protected: true,
  bot_properties: { welcome_text: "Welcome to your new bot" }
});
```

The call schedules or performs an installation flow; it does not return an installed bot object to the next line. Check the intended account and the bot error log for the outcome. Validate all recipient/source input before invoking it.

## Restricted administration methods

| Methods | Role |
| --- | --- |
| `tgSignIn`, `addChildAccount`, `unlinkAllChildAccounts` | Account-linking flows |
| `getParentAccount`, `getParentAccountDetails` | Linked-account information |
| `addExtraPointsToIterationQuota` | Quota adjustment |
| `installPaidStoreBot` | Paid Store installation |
| `resetPassword` | Account recovery through a checked bot-token flow |
| `getAccountBalance` | Account balance lookup |

These methods are not a way for an ordinary bot to grant itself account privileges. Use the existing app/account flows for [linking an account](../account/linked-accounts.md) or [recovering a password](../account/password.md).

A `success` command is used by some restricted methods, but response shapes differ. Do not copy a callback parser between account lookup, quota adjustment, and password reset or display complete responses to chat users.

## Troubleshooting

If a restricted call does nothing, verify that you are using the right API for the task; an ordinary settings form should use `AdminPanel`. For installation, check source-bot access, protection status, recipient details, and any recorded abuse-limit error. Do not bypass a restriction by changing action names or invoking a restricted method through another wrapper.

For ordinary command behavior, continue with [Bot methods](bot.md) and [security](security.md).
