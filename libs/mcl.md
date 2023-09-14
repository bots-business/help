---
description: >-
  This library is used to verify user membership in other channels and chats. It
  is also know as MCLib (MCL)
---

# MembershipChecker (MCL)

{% hint style="success" %}
We have demo bot for MCLib in the Store - BBChannelPromotionBot
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

![](<../.gitbook/assets/image (28).png>)

{% hint style="warning" %}
Please note:&#x20;

* **Small** checking **delay** is not good for iterations
* **One chat** (or one channel) require **1 iteration per check**
{% endhint %}





## Setup

### [**Before all**](../bjs/always-running-commands.md) **command**&#x20;

in @ command:

```javascript
// for automatic checking
// with checking delay from admin panel
Libs.MembershipChecker.handle();
```

{% hint style="warning" %}
This method can have much more iterations usage.&#x20;

You can increase checking delay time for hour and etc



Also you can use it only on /start - but user can leave your channel after joining and bot starting. With @ command it is permanent checking.
{% endhint %}





### Command `/onNeedJoining`

```javascript
let channels = Libs.MembershipChecker.getChats();
Bot.sendMessage("Please join to our channels: " + channels);

```

### Command `/onJoining`:

```javascript
Bot.sendMessage(
    "Thank you for joining to " + 
    options.chat_id
);

isMember = Libs.MembershipChecker.isMember();
let channels = Libs.MembershipChecker.getChats();

if(isMember){
   Bot.sendMessage("you joined to all channels and chats!"
}else{
   Bot.sendMessage("you need to join all channels and chats: " + channels)
}
```

## Limited bot access

If membership is required to use the bot you can use [before all](https://help.bots.business/scenarios-and-bjs/always-running-commands#beforeall-and-afterall-commands) command: `@`

```javascript
// for all chats and channels:
isMember = Libs.MembershipChecker.isMember()

// for one chat / channel:
// isMember = Libs.MembershipChecker.isMember("@chatName")

if(!isMember){
  let channels = Libs.MembershipChecker.getChats();
  Bot.sendMessage("Please join to our channels " + channels)
  return // return from execution
}

```

## Check button

You can also perform manual check if you need something like "check" button:

```javascript
// for all chats and channels:
// this method perform checking without delay
// but not more often than once every 2 seconds
Libs.MembershipChecker.check()
```



## All methods

| Method                | Description                                                                                                                                                                                               | Background |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `setup()`             | install Admin Panel for Lib                                                                                                                                                                               |            |
| `check()`             | check memberships                                                                                                                                                                                         | **+**      |
| `handle()`            | <p>check memberships with delay (you can setup delay in Admin Panel). <br><br>Use this method in <a href="../bjs/always-running-commands.md#beforeall-and-afterall-commands">before all</a> @ command</p> | **+**      |
| `isMember(chat_id)`   | <p>Returns true if the user has joined all resources. chat_id - can be null (it will be all chats)<br><br>Before you need to execute handle() or check()</p>                                              |            |
| `getChats()`          | Returns all resources (group chats, channels) specified in the Admin Panel                                                                                                                                |            |
| `getNotJoinedChats()` | Returns all resources (group chats, channels) specified in the panel that the user has not yet joined                                                                                                     |            |



{% hint style="info" %}
All background methods require additional iterations.
{% endhint %}
