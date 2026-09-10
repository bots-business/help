---
description: Build a valid Bots.Business Git repository with bot.json, command headers, folders and JavaScript libraries.
---


# Bot repository format

A bot repository contains configuration, command files and optional libraries. Use an export as the starting point when moving an existing bot.

{% code title="Example · Example 1" overflow="wrap" %}
```text
bot.json
commands/
  _start.js
  support/
    _help.js
libs/
  Greetings.js
```
{% endcode %}

## bot.json

The root file is required and must be valid JSON:

{% code title="bot.json · Example 2" overflow="wrap" %}
```json
{
  "bb_sync_version": "1.0",
  "name": "ExampleBot",
  "csv_url": null
}
```
{% endcode %}

The importer requires `bb_sync_version` to equal `"1.0"`, then updates the bot's `name` and `csv_url`. It does not import the Telegram token from this file. An export may include `git_remote`, but the import repository is chosen before this file is read; do not treat `git_remote` here as a redirect to another repository.

## Command files

Files belong directly in `commands/` or one folder beneath it. The Git importer does not recursively scan arbitrary directory depth.

`commands/_start.js`:

{% code title="Command files · Example 3" overflow="wrap" %}
```javascript
/*CMD
  command: /start
  help: Start the example bot
  need_reply: false
  aliases: /hello
CMD*/

Bot.sendMessage("Welcome!");
```
{% endcode %}

Without a `command` header, the filename determines the command: an initial underscore becomes `/`. `_start.js` therefore means `/start`; `hello.js` means `hello`. An explicit header takes precedence.

Supported header keys are `command`, `aliases`, `help`, `need_reply`, `auto_retry_time`, `folder`, `answer`, `keyboard` and `group`. Values use `key: value`. Use the exact exported format instead of adding arbitrary YAML or JSON metadata.

For a multiline answer:

{% code title="Command files · Example 4" overflow="wrap" %}
```javascript
/*CMD
  command: /help
  <<ANSWER
Send /start to begin.
Send /help to see this message again.
  ANSWER
CMD*/
```
{% endcode %}

This command replies through Answer and has no BJS body. A command with both Answer and BJS can produce both outputs, depending on its execution path; choose deliberately.

The parent directory supplies a command folder unless `folder` is explicitly set. Keep a `command` key in files using metadata to avoid accidental filename/header ambiguity. Groups determine allowed chat contexts; they are different from folders used for organization.

## Libraries

Place each library at `libs/Name.js`. Import creates an installed library named `Name` from that file. The file must call `publish` to expose its methods:

{% code title="Libraries · Example 5" overflow="wrap" %}
```javascript
function greeting() { return "Hello"; }
publish({ greeting });
```
{% endcode %}

Call it with `Libs.Name.greeting()`. See [Library development](../libraries/development.md) for callbacks and dependencies.

## Check before import

Validate JSON, the supported directory depth, command names, aliases and all library calls. Keep callback commands in the repository too. Review secrets before committing and retain the complete previous export: [Git import](git.md) replaces the destination's commands and installed libraries.

Do not assume VS Code's linked command IDs and the Git import format have identical semantics. An extension's metadata can identify an existing remote command, while a whole Git import recreates the command set.
