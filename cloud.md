# Cloud

#### What it is BB.Cloud? <a id="What-it-is-BB.Cloud?"></a>

BB.Cloud - it is your own server instance. 

We have **monthly** plans of BB.Cloud:

**Nano, Mini, Start, Pro, Business, Big Business** with different power.



## What is different between BB.Cloud and BB.General?

* BB.Cloud have big iterations
* BB.General - all resources are shared into all non cloud bots
* BB.Cloud - all resources are shared only between your bots

## Monthly Plans and specifications for BB.Cloud

| **Cloud** | **Cost, USD** | **Approximate daily iterations** | approx 1 sec iterations |
| :--- | :--- | :--- | :--- |
| Cloud.Nano | 28 | 70 000 | 0.8 |
| Cloud.Mini | 48 | 200 000 | 2.5 |
| Cloud.Start | 95 | 400 000 | 5 |
| Cloud.PRO | 180 | 1 500 000 | 20 |
| Cloud.Business | 270 | 3 500 000 | 40 |
| Cloud.BigBusiness | 400 | 7 000 000 | 80 |

Please note: values may differ slightly, both up and down, depending on the bot's response time, the complexity of your commands, etc.

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

*  `/command1` is **0.1 - 0.2 sec** 
* `/command2` is **1 - 1.5 sec**

{% hint style="info" %}
Bot can only **execute one command** at a time on Nano Cloud
{% endhint %}

So in 1 second on Nano Cloud we can run:

* above 5 times /command1 \(5 iterations per 1 second\)
* 1 time only /command2 \(1 iteration per 1 second\)

{% hint style="info" %}
So Iterations burns depend from your BJS. So we have approximate daily iterations.
{% endhint %}

## Why is my cloud slowing down?

Please look on `/command1` and `/command2` again

`/command1` - can be runned 5 times in 1 second. **Five users** can run this command in one second 

`/command2` - can be runned 1 times in 1 second only. **Only one user** can run this command in one second.

Example. 5 users run `/command1` at the same time:

* bot replied to all users within 1-2 seconds 

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



## **What will happen if my incoming messages exceed the specifications?**

Your bots will run slower. Timeout errors will occur. You need to upgrade your cloud



## **How to determine when it's time to upgrade?**

Look at the Errors tab. If there are a lot of times, this can be a signal.

Contact your admin. It will provide you with complete statistics with graphs.



## How many users can the cloud support?

* 1 user can run 1000 commands per hour
* 1000 users can run 1 command per hour

This is the same use. We have no limit on the count of users.



