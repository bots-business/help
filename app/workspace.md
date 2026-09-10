---
description: Find the right bot and navigate Dashboard, Commands, Admin panel, Properties, Chats, Errors, and Tools in the
  mobile app.
cover: ../.gitbook/assets/cover-app-tools.webp
coverY: 0
layout:
  cover:
    visible: true
    size: hero
---


# Find your bot and its tools

Open **My bots**, then tap a bot to enter its workspace. Check the bot name before changing commands, settings, or data.

<figure><img src="../.gitbook/assets/mobile-my-bots.png" alt="My bots on Android, showing two demonstration bots and their statuses"><figcaption>My bots on Android, showing two demonstration bots and their statuses</figcaption></figure>

## Find the right bot

Use the search to narrow the list by name. Sort and filter controls help find working bots, stopped bots, or bots without tokens. Clear the active filters if a bot seems to be missing.

The list also supports selecting several bots for run/stop or deletion. Review the selected names before confirming a bulk action: deleting a bot is different from stopping it.

<details>
<summary>Keep frequently used bots at the top</summary>

### Keep frequently used bots at the top

Long-press a bot to enter selection mode, select the bots you use often, and choose **Pin selected bots**. Pinned bots have a pin marker and appear before the remaining bots. Select pinned bots and use **Unpin selected bots** to remove the pins.

To reorder pinned bots, stay in selection mode and use their up/down controls. Clear search, sorting and filters if the controls are unavailable. Pinning changes how you find a bot; it does not launch it or copy it.

</details>

## Choose a workspace tab

| Tab | Use it to |
| --- | --- |
| **Dashboard** | Check status, launch or stop, edit name/token, and open the bot in Telegram. |
| [**Commands**](commands.md) | Create answers and BJS, find commands, and organize folders. |
| [**Libraries**](../libraries/README.md) | Inspect and manage the libraries installed in this bot. |
| [**Admin panel**](admin-panel.md) | Fill the management forms provided by the bot developer. |
| [**Properties**](properties.md) | Inspect and edit stored bot or user data. |
| [**Chats**](chats.md) | Find known chats, block/unblock them, and open user properties. |
| [**Broadcast**](broadcasts.md) | Inspect broadcast tasks, audiences, delivery, and progress. |
| [**Errors**](../troubleshooting/errors.md) | Read execution errors and jump to the related command. |
| **Tools** | Import commands, work with Git, and [make an implementation copy](copy-bot.md). |

On a narrow screen, some tab names can be outside the visible part of the tab strip. Scroll the strip to find the remaining tabs. Selecting a tab keeps you within the current bot.

## Launch, stop, and edit

Use **Dashboard → Launch bot** or **Stop bot**. Wait for the status result; a network failure can leave the action unconfirmed. **Open** takes you to Telegram, while **Edit bot** changes the Bots.Business bot configuration.

Stopping and deleting serve different purposes. Stop a bot when you need to investigate behavior; delete only when you no longer need its stored configuration and data.

## App settings and your account

Open [Settings and Lessons](settings-and-lessons.md) for the app theme, language, editor options, training, and support links. [Profile](../account/profile.md) contains account details, email changes, passwords, API access, sessions, and linked Telegram accounts.

App language and editor colors affect your interface. They do not translate saved bot messages or change how BJS executes.

If editing controls are unavailable, check [protected bots](../account/protected-bots.md). To learn the current interface interactively, open **Settings → Lessons**.
