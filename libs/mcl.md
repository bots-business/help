---
description: >-
  This library is used to verify user membership in other channels and chats. It
  is also know as MCLib (MCL)
---

# MembershipChecker (MCL)

{% hint style="success" %}
We have demo bots for MCLib in the Store - [BBJoinBot](https://t.me/BBJoinBot) and [BBChannelPromotionBot](https://t.me/BBChannelPromotionBot)
{% endhint %}

{% hint style="warning" %}
It is recommended to use this library, since it allows you to make the bot work faster.
{% endhint %}

{% hint style="info" %}
Verification methods run in the background so the user does not need to wait for a response from the bot.
{% endhint %}

## Initial setup

Install Library and create `/setup` command:&#x20;

```javascript
Libs.MembershipChecker.setup()
```

Then go to App > Bot > Admin Panels and fill options:

<figure><img src="../.gitbook/assets/изображение (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Please note:&#x20;

* **Small** checking **delay** is not good for iterations
* **One chat** (or one channel) require **1 iteration per check**
* you can turn on "debug info" checkbox for bug searching
{% endhint %}





## Setup

### Callbacks

You need to define commands in the admin panel. You can define only those commands that you need.

| Callback command   | Description                                 |
| ------------------ | ------------------------------------------- |
| `onNeedJoining`    | user still need to join to **any** resource |
| `onNeedAllJoining` | user still need to join to **all** resource |
| `onJoining`        | user just joined to **any** resource        |
| `onAllJoining`     | user just joined to **all** resources       |
| `onStillJoined`    | user is still joined to **all** resources   |
| `onError`          | error callback                              |



[**Before all**](../bjs/always-running-commands.md) **command**

in @ command:

```javascript
// for automatic checking
// with checking delay from admin panel
Libs.MembershipChecker.handle();
```

{% hint style="warning" %}
This method can have much more iterations usage.&#x20;

You can increase checking delay time for hour and etc for decreasing iterations usage.



Also you can use it only on `/start` - but user can leave your channel after joining and bot starting. With @ command it is permanent checking.
{% endhint %}





### Command `/onNeedJoining`

```javascript
Bot.sendMessage(
  "You still need to join: " + options.chat_id
)

// you can access for passed data:
// Bot.inspect(options.bb_option)
```

{% hint style="success" %}
This callback will be executed if the user has not yet joined or has left after `handle()` or `check()` methods
{% endhint %}

{% hint style="info" %}
This callback will be executed **per each channel or chat**. For example, if you have 5 not joined chats you will have 5 callbacks
{% endhint %}

### Command `/onJoining`:

```javascript
if(!options){ return } // protect from manual run
Bot.sendMessage("Thank you for joining to" + options.chat_id);

// you can access for passed data:
// Bot.inspect(options.bb_option)
```

{% hint style="success" %}
This callback will be executed if the user just joined after after `handle()` or `check()` methods
{% endhint %}

{% hint style="info" %}
This callback will be executed if the user **just joined to any channel**
{% endhint %}



### Command `/onAllJoining`:

```javascript
if(!options){ return } // protect from manual run
Bot.sendMessage("Thank you for joining!");

// you can access for passed data:
// Bot.inspect(options.bb_option)
```

{% hint style="success" %}
This callback will be executed if the user just joined after after `handle()` or `check()` methods
{% endhint %}

{% hint style="info" %}
This callback will be executed **once** if the user **has joined all channels**
{% endhint %}

### Command  /onStillJoined

```javascript
Bot.sendMessage(
  "You still have membership in our groups. Thank!"
)
```

{% hint style="success" %}
This callback will be executed if the user **still joined to all** channels and chats after  `check()` method only.
{% endhint %}





## Limited bot access

If membership is required to use the bot you can use [before all](https://help.bots.business/scenarios-and-bjs/always-running-commands#beforeall-and-afterall-commands) command: `@`

```javascript
// for all chats and channels:
let isMember = Libs.MembershipChecker.isMember();

// for one chat / channel:
// isMember = Libs.MembershipChecker.isMember("@chatName")

// we need this commands because user always need
// to /start bot and make "/check" command
const skipCommands = [
   "/start",
   "Check", "/check",
    //  "/setup"   // it is also can be
];

const canRunBot = isMember || skipCommands.includes(message);

if(!canRunBot){
  let channels = Libs.MembershipChecker.getChats();
  Bot.sendMessage("Please join to our channels " + channels)
  return // return from bot execution
}

```

## Check button

You can also perform manual check if you need something like "check" button.&#x20;

<figure><img src="../.gitbook/assets/изображение (1).png" alt=""><figcaption></figcaption></figure>

Example for `/check` command:

```javascript
// for all chats and channels:
// this method perform checking without delay
// but not more often than once every 2 seconds
Libs.MembershipChecker.check()

// also you can pass any data for callbacks:
// Libs.MembershipChecker.check({ any: "data", here: "for callbacks"  })
```

{% hint style="info" %}
It is good to use [Cooldown Lib](cooldown-lib.md) here

User can press button many times. So we can restrict this and save iterations
{% endhint %}

##

## All methods

| Method                | Description                                                                                                                                                                                                                                                                                        | Background |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `setup()`             | Install Admin Panel for Lib                                                                                                                                                                                                                                                                        |            |
| `check(options)`      | <p>Force check memberships. </p><p></p><p>You can pass any data in options for callbacks (<code>onJoining</code>, <code>onNeedJoining</code>, <code>onNeedAllJoining</code>, <code>onStillJoined</code>)</p>                                                                                       | **+**      |
| `handle(options)`     | <p>Soft check memberships with delay (you can setup delay in Admin Panel). <br><br>Use this method in <a href="../bjs/always-running-commands.md#beforeall-and-afterall-commands">before all</a> @ command<br><br>You can pass any data in options for callbacks (onJoining and onNeedJoining)</p> | **+**      |
| `isMember(chat_id)`   | <p>Returns true if the user has joined all resources. chat_id - can be null (it will be all chats)<br><br>Before you need to execute handle() or check()</p>                                                                                                                                       |            |
| `getChats()`          | Returns all resources (group chats, channels) specified in the Admin Panel                                                                                                                                                                                                                         |            |
| `getNotJoinedChats()` | Returns all resources (group chats, channels) specified in the panel that the user has not yet joined                                                                                                                                                                                              |            |



{% hint style="info" %}
All background methods require additional iterations.
{% endhint %}
