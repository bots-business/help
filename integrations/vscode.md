---
description: Edit Bots.Business command files in VS Code and understand when saving changes the remote bot.
---


# Edit bot commands in VS Code

The official Bots.Business extension uploads linked command files when you save them. Use a test bot while setting up the workspace: saving is a remote change, not just a local draft.

{% stepper %}

{% step %}

### Connect a workspace

1. Install [Bots.Business from Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=bots-business.bots-business).
2. Open a dedicated folder in VS Code.
3. Run **Bots.Business: Login** from the Command Palette and authenticate with your [BB account's API access](../account/profile.md#find-the-right-access-control).
4. Run **Bots.Business: Set Bot ID** and select the intended bot.
5. Use the extension's bot/file actions to populate the workspace, then inspect the selected bot ID before editing.

The current extension uses files under `commands/**/*.js` and stores remote command IDs in CMD metadata. Preserve those IDs when editing an existing command. Do not reuse a linked workspace for another bot by manually copying its IDs.

{% endstep %}

{% step %}

### Make one change

Open an existing command file, change one reply, save, and test that command in Telegram. Check the extension's output for API failures. A file saved on disk can still have failed to upload.

If the result is correct, commit the local change to Git. Choose one active editor for the same commands so an older mobile or desktop edit does not overwrite a newer version.

{% endstep %}

{% endstepper %}

## Import, deletion and unlinking

Use the extension's explicit create/delete/unlink actions and read their confirmation. File deletion and unlinking can affect command associations; test them on a disposable bot before reorganizing a working workspace.

This workflow is different from [whole-bot Git import](git.md), which replaces the destination command and library set. Keep an export before changing workflows. See [repository format](repository-format.md) for the ordinary Git import files.

If saving does nothing, check authentication, the linked bot ID, the file's location and its metadata. For version-specific commands, use the installed extension's Command Palette and the official listing rather than guessing an old `BB:*` command name.
