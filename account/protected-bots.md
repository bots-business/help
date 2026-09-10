---
description: Learn what a protected bot lets its owner manage, why command editing may be unavailable, and how to configure
  it through an Admin panel.
---


# Work with a protected bot

{% hint style="info" %}
A protected bot limits access to its implementation. You can use the bot and the management features it exposes, while command viewing or editing can be unavailable.
{% endhint %}

## Configure the bot

Start with **Admin panel** and the installation instructions supplied by its developer. A panel can expose the settings needed to operate the bot without making its commands editable.

The current mobile app allows management screens such as Dashboard, Admin panel, Properties, Chats, and Errors. It blocks command editing and copying/export actions for a protected bot. CSV upload is also blocked in the current Tools interface. Check the available actions before following a guide written for an unprotected bot.

Git import is a different operation from export. Follow the [Git guide](../integrations/git.md) and the developer's update instructions; importing can replace the existing implementation.

## “I cannot open Commands”

Check whether the bot is protected. Reinstalling the app or searching for another editor does not grant permission to read its code. Contact the bot developer if the provided management controls do not support the change you need.

For an ordinary unprotected bot, use [command editing](../app/commands.md).

## Guidance for bot developers

Provide an [Admin panel](../bjs/admin-panel.md), clear setup steps, and understandable errors for the owner. Test the installed bot as that owner, including the actions they cannot access.

Protection is not a secret-storage boundary. The owner may inspect properties, visible panel values, and error messages. Do not embed your personal API credentials where the recipient can read them. Installation and copying options are described in [BBAdmin](../bjs/bb-admin.md).
