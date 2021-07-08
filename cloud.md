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

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Cloud</b>
      </th>
      <th style="text-align:left"><b>Cost, USD</b>
      </th>
      <th style="text-align:left">
        <p><b>Approximate</b>
        </p>
        <p><b>daily iterations</b>
        </p>
      </th>
      <th style="text-align:left">
        <p><b>Approximate</b>
        </p>
        <p><b>1 sec iterations</b>
        </p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Cloud.Hobby<b>*</b>
      </td>
      <td style="text-align:left">15</td>
      <td style="text-align:left">70 000</td>
      <td style="text-align:left">0.8</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Nano</td>
      <td style="text-align:left">28</td>
      <td style="text-align:left">100 000</td>
      <td style="text-align:left">1.15</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Mini</td>
      <td style="text-align:left">48</td>
      <td style="text-align:left">200 000</td>
      <td style="text-align:left">2.5</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Start</td>
      <td style="text-align:left">95</td>
      <td style="text-align:left">400 000</td>
      <td style="text-align:left">5</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.PRO</td>
      <td style="text-align:left">180</td>
      <td style="text-align:left">1 500 000</td>
      <td style="text-align:left">20</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Business</td>
      <td style="text-align:left">270</td>
      <td style="text-align:left">3 500 000</td>
      <td style="text-align:left">40</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.BigBusiness</td>
      <td style="text-align:left">400</td>
      <td style="text-align:left">7 000 000</td>
      <td style="text-align:left">80</td>
    </tr>
  </tbody>
</table>

Please note: values may differ slightly, both up and down, depending on the bot's response time, the complexity of your commands, etc.

{% hint style="info" %}
\*Cloud.Hobby do not have [brodcasting](scenarios-and-bjs/message-broadcasting.md#do-you-want-broadcast-text) and [auto retry](commands/auto-retry.md) for commands.
{% endhint %}

## How does the cloud work?

The cloud consists of 6 components - tasks:

* WEB task \(processes requests from telegram\)
* BJS-runtime task \(process BJS\)
* other 4 tasks for command repetition, for performing background tasks and for brodcasting

{% hint style="info" %}
There are only 4 tasks in the Hobby Cloud, not 6.
{% endhint %}

Depending on the tariff plan, the process count changes for WEB and BJS-runtime.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Plan</b>
      </th>
      <th style="text-align:left"><b>Web processes</b>
      </th>
      <th style="text-align:left">
        <p><b>BJS-runtime</b>
        </p>
        <p><b>processes</b>
        </p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Cloud.Hobby</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Nano</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">2</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Mini</td>
      <td style="text-align:left">2</td>
      <td style="text-align:left">2</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Start</td>
      <td style="text-align:left">6</td>
      <td style="text-align:left">3</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.PRO</td>
      <td style="text-align:left">10</td>
      <td style="text-align:left">5</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.Business</td>
      <td style="text-align:left">20</td>
      <td style="text-align:left">10</td>
    </tr>
    <tr>
      <td style="text-align:left">Cloud.BigBusiness</td>
      <td style="text-align:left">40</td>
      <td style="text-align:left">20</td>
    </tr>
  </tbody>
</table>

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

### Other slowdown issues

Unfortunately, bad code can lead to slowdowns. Check out this [example](iterations.-how-to-reduce-theys.md#beware-of-endless-loops) and also [this](iterations.-how-to-reduce-theys.md#beware-of-big-loops). 

## **What will happen if my incoming messages exceed the specifications?**

Your bots will run slower. Timeout errors will occur. You need to upgrade your cloud



## **How to determine when it's time to upgrade?**

Look at the Errors tab. If there are a lot of times, this can be a signal.

Contact your admin. It will provide you with complete statistics with graphs.



## How many users can the cloud support?

* 1 user can run 1000 commands per hour
* 1000 users can run 1 command per hour

This is the same use. We have no limit on the count of users.



