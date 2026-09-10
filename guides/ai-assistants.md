---
description: Give an AI assistant the Bots.Business documentation and ask for BJS examples with the correct runtime, command context, and callbacks.
---

# Use this help with an AI assistant

Give your assistant the relevant Bots.Business pages before asking it to write BJS. Include the command's trigger, the expected result, and the libraries installed in your bot.

## Read the documentation

The published help offers a [documentation index](https://help.bots.business/llms.txt), a [full-text version](https://help.bots.business/llms-full.txt), and Markdown pages. Use the page actions when your reader exposes a Markdown or AI-copy option.

For clients that support a remote documentation MCP server, the help endpoint is:

```text
https://help.bots.business/~gitbook/mcp
```

Its role is to search and read published documentation. It is different from [Bots.Business MCP](../integrations/mcp.md), which connects to an account and can expose product operations. Reading help does not require giving an assistant your bot token or account API key.

## Start with a specific request

```text
Use the current Bots.Business help to create a /name command.
It should ask for a name, wait for a text reply, and save that name
as a user property. Include the command fields, complete BJS,
a separate /cancel command, expected results, and source links.
Use Bots.Business BJS, including its actual runtime limitations.
```

For other tasks, state whether the command runs from a user message, Auto Retry, HTTP callback, webhook, or delayed job. That determines which values and methods are available.

## Check the answer before using it

- Follow its source links and check the exact method names and parameter types.
- Require all callback commands, not just the code that starts an operation.
- Check user, chat, and ID scope. Scheduled work may have no incoming user or chat.
- Check library installation and the documented provider version for integrations.
- Run a small example in a test bot, including the failure and cancellation paths.

Do not treat a syntax check as an execution test. A plausible-looking method from Node.js, a different Telegram library, or an old help page may not work in BJS.

## Useful starting pages

[Execution context](../bjs/context.md), [Bot](../bjs/bot.md), [HTTP](../bjs/http.md), [properties](../bjs/user-properties.md), [AdminPanel](../bjs/admin-panel.md), and [errors](../bjs/errors.md) explain common sources of incorrect generated examples.

If the assistant cannot read an endpoint, give it the relevant Markdown or page text. State the missing source instead of letting it infer an undocumented method.
