# Bot Management Tools

This page provides a suite of tools to manage and customize your bot.  You can upload commands, create copies, and synchronize with Git repositories.

## 1. Upload Commands 

![Upload Commands](/.gitbook/assets/section-tools.png) 

Easily add new functionalities to your bot by uploading commands.  If your data is in a Google Sheet, you can directly create a bot from it!  Simply provide the CSV URL and click "Upload".

[Read Here](/create-bot-from-google-table.md) to know more about uploading commands and supported formats. 


## 2. Make Bot Copy

![Make Bot Copy](/.gitbook/assets/copy-bot.png)

Need to experiment with new features or save a version of your bot?  Use the "Make Copy" function to duplicate your current bot configuration. This allows you to modify the copy without affecting the original.


## 3. Git Sync (Import/Export)

![Git Sync](/.gitbook/assets/git-sync.png)

Integrate your bot with a Git repository for version control and collaboration.  

* **Deploy Key:** `ssh-ed25519 AAAAC3NA...` (This is your unique deploy key for Git access)
* **Git Repository:**  (Enter the URL of your Git repository here)

You can:

* **Export to Git:**  Push your bot's current configuration to your Git repository.
* **Import from Git:**  Load a bot configuration from your Git repository.

[Read Here](/git) to know more about Git integration, best practices, and troubleshooting.
