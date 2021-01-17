# Iterations. How to reduce theys?

All iterations on Bots.Business are paid. You can choose payment plan with different iterations count.

## What it is - "iteration"?

Each payment plan has its own iteration limit.

**1 iteration it is:**

* income message to bot
* [Bot.runCommand](https://help.bots.business/scenarios-and-bjs/bot-functions)\(command\) or `Bot.run` in the BJS spend 1 iteration
* Pressing the keyboard button
* Pressing the inline keyboard button
* 1 Auto Retry - it is 1 iteration
* 1 received webhook - it is 1 iteration
* **5 chats** in [`Bot.runAll`](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command - 1 iteration
* **5** **sended message** on mass broadcasting - 1 iteration
* **5** **chats** on [Information refreshing](https://help.bots.business/bot-information) \(in Bot dashboard\) - spend 1 iteration
* **100 incoming messages** in blocked chat with method `Bot.blockChat(chat.id)`



5 iterations it is:

* [CSV import](https://help.bots.business/create-bot-from-google-table) - spend 5 iterations
* Git [import](https://help.bots.business/git/import-bot-from-git-repository)/[export](https://help.bots.business/git/export-bot-to-git-repository) - spend 5 iterations



{% hint style="info" %}
Single command execution - 1 iteration
{% endhint %}

{% hint style="success" %}
Iterations are restored every month. Each payment plan has its own iteration limit.
{% endhint %}

### Extra Points

* If you do not have enough iterations, Extra Points are spent.
* Unused Extra Points remain for the next month.
* You can get Extra Points for 💎 BB Points or [buy them](https://t.me/BotsBusinessAdminBot).



### How to get more iterations?

You can upgrade your Plan, buy Extra Points or order BB Cloud with unlimited iterations.



## How to reduce iterations count?

Reduce income messages to bot:

* remove it from super groups
* remove un useful commands

Reduce Bot.runCommand in BJS

Reduce [Auto Retry](https://help.bots.business/commands/auto-retry) calls

### Beware of endless loops

Use `Bot.runCommand`, `Bot.run`, `Bot.runAll` carefully. 

**Example 1**

Bad code example. Command `/check`

```javascript
Bot.runCommand("/task")
```

Command `/task`

```javascript
...
Bot.runCommand("/check")
...
```

So we have now scheduled `/task` in `/check`. But /task also have run for `/check`

You will end up in an infinite loop and your iterations **will quickly end**

**Example 2**

Bad code example. Command `/check`

```javascript
Bot.run(command: "/task", run_after: 60*60)  // 1 hour delay


```

Command `/task`

```javascript
...
Bot.runCommand("/check")
...
```

So we have now scheduled `/task` in `/check` with delay for 1 hour. But /task also have run for `/check`

But here we have delay for 1 hour. So we have 1 task per hour \(and per user!\). Is it good?

No! Because user can run `/check` command several times. For example 6 times in one minute. You will be have 6 background tasks during 1 hour instead of 1 tasks. Also any users can execute this command for several times.

You will end up in an infinite loop and your iterations **will quickly end**

{% hint style="danger" %}
Be very careful with Bot.run methods
{% endhint %}

\*\*\*\*

