---
description: Find Bots.Business recipes for buttons, user input, properties, referrals, channel membership, HTTP, payments,
  and scheduled work.
---


# Find a recipe for your bot

For a guided sequence with checkpoints, choose a [Tutorial](../tutorials/README.md). Use the recipes below when you need one specific task.

{% hint style="info" %}
Choose the result you want, then follow the complete guide. Each linked guide explains its required commands, context, and limitations.
{% endhint %}

## Messages and menus

| Task | Guide |
| --- | --- |
| Reply to `/start` without code | [Create your first bot](../start/first-bot.md) |
| Show a menu of text buttons | [Reply keyboard](../app/reply-keyboard.md) |
| Put a changing balance on a reply button | [Dynamic balance keyboard](../app/reply-keyboard.md#show-a-changing-balance-on-a-button) |
| Build a menu in two languages | [SmartBot menu](smartbot-menu.md) |
| Put buttons under a message or edit a sent message | [Bot methods](../bjs/bot.md) |
| Receive and send back a photo or document, with a button | [Send photos and documents](send-media.md) |
| Call another Telegram method | [Telegram API](../bjs/telegram-api.md) |
| Put a saved value in an ordinary Answer | [Answer placeholders](../app/commands.md#put-a-saved-property-in-an-answer) |
| Receive contacts, locations, photos or group events | [Telegram updates](telegram-updates.md) |
| Reply to a particular incoming message | [Reply with Telegram API](../bjs/telegram-api.md#reply-to-the-users-message) |
| Delete a bot message after a delay | [Temporary messages](../bjs/telegram-api.md#delete-a-bot-message-after-a-delay) |
| Answer an inline query | [Inline bots](../bjs/inline.md) |
| Open a form inside Telegram and receive its result | [Telegram Mini App form](mini-app.md) |

## User data and access

| Task | Guide |
| --- | --- |
| Ask a question and save the reply | [Collect input](../app/collect-input.md) |
| Accept a whole-number quantity and repeat invalid input | [Quantity dialog](quantity-dialog.md) |
| Keep a personal learning checklist between visits | [Task progress](task-checklist.md) |
| Keep a counter or setting between commands | [User and bot properties](../bjs/user-properties.md) |
| Read a collection of stored entries in pages | [Lists](../bjs/lists.md) |
| Move a JSON array into individual List entries | [Array migration](../bjs/lists.md#migrate-a-json-array-into-a-list) |
| Read or update another known user or owned bot | [Property scopes](../bjs/user-properties.md#object-form-and-another-user) |
| Check channel membership | [MembershipChecker](../libraries/membership.md) |
| Track referral links | [Referrals](../libraries/referrals.md) |
| Manage a balance or resource | [Resources](../libraries/resources.md) |
| Expose settings to a bot owner | [Create an Admin Panel](../bjs/admin-panel.md) |

## Automation and services

| Task | Guide |
| --- | --- |
| Run a command after a delay | [Scheduled reminder and cancellation](../bjs/background.md#a-complete-reminder) |
| Remind a user after they stop interacting | [Inactivity reminder](inactivity-reminder.md) |
| Run a command periodically | [Auto Retry setup and stopping](../bjs/background.md#auto-retry-run-periodically) |
| Send to many known chats | [BJS broadcasts](../bjs/broadcasts.md) |
| Request data from another service | [HTTP requests](../bjs/http.md) |
| Receive a webhook | [Webhook library](../libraries/webhooks.md) |
| Create an OxaPay test invoice | [OxaPay](../integrations/oxapay.md) |
| Use OxaPay white-label, payout or swap operations | [OxaPay V1 operations](../integrations/oxapay-operations.md) |
| Migrate an existing GoogleApp or GoogleTableSync connector | [Google Sheets integration](../integrations/google-apps-script.md) |
| Import commands from a spreadsheet | [Google Sheets or CSV import](../integrations/google-table-import.md) |
| Pass a campaign value in a start link | [Deep links](deep-links.md) |

If an example is not working, start with its stated context and required libraries. For an error message, use [Errors and debugging](../troubleshooting/errors.md); for generated code, check [using an AI assistant](ai-assistants.md).
