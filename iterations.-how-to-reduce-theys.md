# Iterations. How to reduce theys?

All iterations on Bots.Business are paid. You can choose payment plan with different iterations count.

## What it is - "iteration"?

Each payment plan has its own iteration limit.

**1 iteration it is:**

* income message to bot
* [Bot.runCommand](https://help.bots.business/scenarios-and-bjs/bot-functions)(command) or `Bot.run` in the BJS spent 1 iteration
* `Bot.run` with `run_after` spent 5 iterations
* Pressing the keyboard button
* Pressing the inline keyboard button
* 1 Auto Retry - it is 1 iteration
* 1 received webhook - it is 1 iteration
* **1 chats** in [`Bot.runAll`](https://help.bots.business/scenarios-and-bjs/bot-functions#bot-runall-options) command - 1 iteration
* 1 **sended message** on mass broadcasting - 1 iteration
* **5** **chats** on [Information refreshing](https://help.bots.business/bot-information) (in Bot dashboard) - spend 1 iteration
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

## Beware of endless loops

Use `Bot.runCommand`, `Bot.run`, `Bot.runAll` carefully.&#x20;

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

But here we have delay for 1 hour. So we have 1 task per hour (and per user!). Is it good?

No! Because user can run `/check` command several times. For example 6 times in one minute. You will be have 6 background tasks during 1 hour instead of 1 tasks. Also any users can execute this command for several times.

You will end up in an infinite loop and your iterations **will quickly end**

{% hint style="danger" %}
Be very careful with Bot.run methods
{% endhint %}



## Beware of **big** loops

Very bad example:

![](<.gitbook/assets/image (54).png>)

Command `check:`

```javascript
var user = options.result.status
User.setProperty("status", user, "string")
if ((user == "member") | (user == "administrator") | (user == "creator")) {
  Bot.runCommand("join2")
  User.addToGroup("user")
}

if (user == "left") {
  Bot.sendMessage("*⚠️ Not Joined :- @Kjtricks_Official *")
}
```

Command `join2`

```javascript
var channel = "@MyChanell1"
let id = user.telegramid;

Api.getChatMember({ chat_id: channel, user_id: id, on_result: "check2" })
```



\
Command `check2`

```javascript
var user = options.result.status
User.setProperty("status", user, "string")
if ((user == "member") | (user == "administrator") | (user == "creator")) {
  Bot.runCommand("join3")
  User.addToGroup("user")
}

if (user == "left") {
  Bot.sendMessage("*⚠️ Not Joined :- @Kjtricks_Official *")
}
```

and etc!

`join1 > check1 > join2 > check2 > ....  join10 >  check10`

**What is problem?**

* each Api.getChatMember spent 1 - 3 sec for execution
* Bot.runCommand run new BJS immediately!

We have 10 join + 10 check. So it will be 10 - 30 secs per 1 message from 1 user.&#x20;

* On Nano Cloud second user must wait this 30 secs! Also even Business Cloud is completely down!
* each Bot.runCommand burn 1 iterations. We have 20 iterations here!

**Fix**

* Use [MCLib](libs/mcl.md)
* Use [Bot.run](bjs/bot-functions.md#bot-run-params) with run\_after. It run task in background (Users don't have to wait)
* Do not use Bot.run in chain. It is not good.&#x20;

&#x20;
