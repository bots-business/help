---
description: Use this extension for quickly bot development with Bots.Business
---

# VS Code Extension

Use the Bots.Business VS Code extension to develop and maintain your bots directly from the file system, with automatic synchronization to the Bots.Business platform.

### What this extension does

This version of the extension is **file-first**:

* The bot is downloaded into your workspace as normal files.
* All bot commands live in `commands/**/*.js` – these files are the **source of truth**.
* When you save a file, the extension automatically sends the updated command to Bots.Business.
* You can use all VS Code features (AI assistants, Git, search, refactoring, etc.) on your bot code.

### Installation

You can install the extension from:

* **Visual Studio Marketplace** – search for `Bots.Business` in the Extensions view, or use the direct link from the Bots.Business site.
* **Open VSX** (for VSCodium and compatible editors) – search for `bots-business`.

After installation, reload VS Code if it doesn’t prompt you automatically.

### Requirements

* A Bots.Business account.
* Your **BB API Key** from the Bots.Business app (Profile section).
* VS Code / VSCodium with JavaScript support enabled.

### Connecting VS Code to Bots.Business

1. Open VS Code with any folder as your workspace (it can be empty for a new bot).
2. Open the **Command Palette** (`Ctrl+Shift+P` / `⌘+Shift+P`).
3. Run **`BB:login`** (or **“Bots.Business: Login”** depending on your UI).
4. Paste your **BB API Key** from the Bots.Business Profile page and confirm.
5. Optionally, you can also set the key in VS Code settings:
   * `File → Preferences → Settings → Extensions → Bots.Business`
   * Or directly in `settings.json` via `"bots-business.apiKey"`.

Once the API key is saved, the extension can access your bots.

### Downloading a bot to the file system

1. Make sure you are logged in via `BB:login`.
2. Open the Bots.Business view or run the relevant command from the Command Palette (for example, a bot install / download command).
3. Select the bot you want to work with.
4. The extension will create a local structure in your workspace, including a `commands/` folder with one `.js` file per command.

Each command now appears as a regular JavaScript file in the VS Code Explorer.

### File-first workflow

#### Editing existing commands

1. Open any file under `commands/`, e.g. `commands/start.js`.
2. Change the code as needed.
3. **Save the file** (`Ctrl+S` / `⌘+S`).

On save:

* The extension detects the change.
* The corresponding command is automatically updated on Bots.Business via the API.
* The file in your repository / project remains the primary source.

You can now freely use:

* AI code assistants in VS Code
* Refactoring tools
* Multi-file search and replace
* Git / other VCS tools

#### Creating new commands

There are two main options:

1. **Via extension command (recommended)**
   * Use the Command Palette and run the “new command” action (e.g. `BB:newCommand`).
   * The extension creates a new file in `commands/` with the correct internal metadata.
2. **From a new file**
   * Create a new `.js` file under `commands/`.
   * Use the extension’s command to register / sync this file as a new command (exact command name depends on the current version).
   * After the first sync, the command will exist on Bots.Business and will be linked to this file.

#### Deleting commands

Typical flow:

* Delete the corresponding file in `commands/`.
* Run the relevant extension command or refresh (e.g. `BB:refresh`) so the tree and remote state are updated.

If deletion behavior is critical for your workflow, check current release notes or test on a non-production bot first.

### CMD-blocks and metadata

Each command file contains an internal **CMD-block** – a special comment block with metadata, including the command ID on Bots.Business.

* The CMD-block is used by the extension to match a file to a remote command.
* Do **not** remove this block.
* Do **not** manually edit its contents unless you know exactly what you are doing.

If the CMD-block is broken or removed, the extension may not be able to sync the file with the existing command and can treat it as a different/new command.

### Synchronization rules (summary)

* **Source of truth**: `commands/**/*.js` in your workspace.
* **Direction**: local file → Bots.Business on every save.
* **Initial import**: when you first download a bot, its current commands are written into files.
* **Conflicts**: if you edit commands both in the web UI and in VS Code, the last saved version wins. For consistent workflow, prefer editing through the extension once the bot is file-based.

### Typical quick-start scenario

1. Install the extension from Marketplace/Open VSX.
2. Get your BB API Key from the Bots.Business Profile.
3. Run `BB:login` in VS Code and paste the key.
4. Download a bot into an empty folder.
5. Open `commands/start.js`, change a reply, save the file.
6. Test the bot in Telegram – the reply is updated.

### Troubleshooting

* **Nothing happens on save**
  * Check that you are logged in (`BB:login`).
  * Confirm the workspace contains the bot files and `commands/` folder.
  * Make sure the file is under `commands/` and has a CMD-block.
* **Bot tree looks outdated in VS Code**
  * Use the refresh command (e.g. `BB:refresh`) to reload data from Bots.Business.
* **API errors**
  * Verify the API key is valid and not expired.
  * Check that you still have access to the selected bot in the web interface.

For advanced usage, refer to the extension’s changelog and repository, or test changes on a non-production bot first.



## Links

You can istall extension from the Stores or from VSCode.

{% embed url="https://marketplace.visualstudio.com/items?itemName=bots-business.bots-business" %}

{% embed url="https://open-vsx.org/extension/bots-business/bots-business" %}

