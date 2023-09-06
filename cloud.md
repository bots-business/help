# Cloud

#### What it is BB.Cloud? <a href="#what-it-is-bb.cloud" id="what-it-is-bb.cloud"></a>

BB.Cloud - it is your own server instance.&#x20;

We have **monthly** plans of BB.Cloud:

**Nano, Mini, Start, Pro, Business, Big Business** with different power.



## What is different between BB.Cloud and BB.General?

* BB.Cloud have big iterations
* BB.General - all resources are shared into all non cloud bots
* BB.Cloud - all resources are shared only between your bots

## Monthly Plans and specifications for BB.Cloud

<figure><img src=".gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

| **Cloud**         | **Cost** | **Monthly iterations** |
| ----------------- | -------- | ---------------------- |
| Cloud.Hobby**\*** | $22      | 1 million              |
| Cloud.Nano        | $33      | 2 million              |
| Cloud.Mini        | $55      | 5 million              |
| Cloud.Start       | $99      | 10 million             |
| Cloud.PRO         | $220     | 45 million             |
| Cloud.Business    | $300     | 100 million            |
| Cloud.BigBusiness | $440     | 200 million            |



<figure><img src=".gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

| **Cloud**         | **Approximate iterations:** |             |          |
| ----------------- | --------------------------- | ----------- | -------- |
|                   | **daily**                   | **minutes** | **COST** |
| Cloud.Hobby**\*** | 28 000                      | 20          | $22      |
| Cloud.Nano        | 60 000                      | 45          | $33      |
| Cloud.Mini        | 140 000                     | 100         | $55      |
| Cloud.Start       | 300 000                     | 220         | $99      |
| Cloud.PRO         | 1 400 000                   | 1000        | $220     |
| Cloud.Business    | 3 000 000                   | 2200        | $300     |
| Cloud.BigBusiness | 6 200 000                   | 4500        | $440     |

Please note: values may differ slightly, both up and down, depending on the bot's response time, the complexity of your commands, etc.

{% hint style="info" %}
\*Cloud.Hobby do not have [brodcasting](bjs/message-broadcasting.md#do-you-want-broadcast-text) and [auto retry](commands/auto-retry.md) for commands.
{% endhint %}

##

## How does the cloud work?

The cloud consists of 6 components - tasks:

* WEB task (processes requests from telegram)
* BJS-runtime task (process BJS)
* other 4 tasks for command repetition, for performing background tasks and for brodcasting

{% hint style="info" %}
There are only 4 tasks in the Hobby Cloud, not 6.
{% endhint %}

Depending on the tariff plan, the process count changes for WEB and BJS-runtime.

| **Plan**          | **Web processes** | <p><strong>BJS-runtime</strong></p><p><strong>processes</strong></p> |
| ----------------- | ----------------- | -------------------------------------------------------------------- |
| Cloud.Hobby       | 1                 | 1                                                                    |
| Cloud.Nano        | 1                 | 2                                                                    |
| Cloud.Mini        | 2                 | 2                                                                    |
| Cloud.Start       | 6                 | 3                                                                    |
| Cloud.PRO         | 10                | 5                                                                    |
| Cloud.Business    | 20                | 10                                                                   |
| Cloud.BigBusiness | 40                | 20                                                                   |

## **Why we have approximate daily iterations?**

As a rule 1 command execution spent 1 iteration.

For example - `/command1`:

```javascript
Bot.sendMessage("Hello");  // it is take above 100 ms for execution
// Total time: 100 ms
```

and `/command2`:

```javascript
// bot send 10 messages with "Hello"
Bot.sendMessage("Hello-1");  // 100 ms
Bot.sendMessage("Hello-2");  // + 100 ms
Bot.sendMessage("Hello-3");  // + 100 ms
...
Bot.sendMessage("Hello - 10"); // + 100 ms
// Total time: 1000 ms = 1 sec
```

`/command1` and `/command2` spent only 1 iteration. But the second command takes **ten times as long** to execute.

Approx execution time for:

* &#x20;`/command1` is **0.1 - 0.2 sec**&#x20;
* `/command2` is **1 - 1.5 sec**

{% hint style="info" %}
Bot can only **execute one command** at a time on Nano Cloud
{% endhint %}

So in 1 second on Nano Cloud we can run:

* above 5 times /command1 (5 iterations per 1 second)
* 1 time only /command2 (1 iteration per 1 second)

{% hint style="info" %}
So Iterations burns depend from your BJS. So we have approximate daily iterations.
{% endhint %}

## Why is my cloud slowing down?

Please look on `/command1` and `/command2` again

`/command1` - can be runned 5 times in 1 second. **Five users** can run this command in one second&#x20;

`/command2` - can be runned 1 times in 1 second only. **Only one user** can run this command in one second.

Example. 5 users run `/command1` at the same time:

* bot replied to all users within 1-2 seconds&#x20;

5 users run `/command2` at the same time:

* 1 user have answer after 1 second
* 2 user have answer after 2 seconds
* 3 user have answer after 3 seconds
* last user have answer only after 10 seconds!

**How to fix this?**

* Don't use too many Bot.sendMessage, Bot.sendKeyboard and etc in one command
* Don't use too many Bot.runCommand or Bot.run in one command or on chain
* Don't use too many Api.xxx methods

Make small commands. Use background tasks with Bot.run and run\_after

### Other slowdown issues

Unfortunately, bad code can lead to slowdowns. Check out this [example](iterations.-how-to-reduce-theys.md#beware-of-endless-loops) and also [this](iterations.-how-to-reduce-theys.md#beware-of-big-loops).&#x20;

## **What will happen if my incoming messages exceed the specifications?**

Your bots will run slower. Timeout errors will occur. You need to upgrade your cloud



## **How to determine when it's time to upgrade?**

Look at the Errors tab. If there are a lot of times, this can be a signal.

Contact your admin. It will provide you with complete statistics with graphs.



## How many users can the cloud support?

* 1 user can run 1000 commands per hour
* 1000 users can run 1 command per hour

This is the same use. We have no limit on the count of users.

