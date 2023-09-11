# MembershipChecker (MCL)

This library is used to verify user membership in other channels and chats. It is also know as **MCLib (MCL)**

{% hint style="success" %}
It is recommended to use this library, since it allows you to make the bot work faster
{% endhint %}

## Initial setup

Install Library and create `/setup` command:&#x20;

```javascript
Libs.MembershipChecker.setup()
```

Then go to App > Bot > Admin Panels and fill options:

![](<../.gitbook/assets/image (28).png>)

{% hint style="warning" %}
Please note: small checking delay is not good for iterations
{% endhint %}

On [before all](../bjs/always-running-commands.md) command @:

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





Command `/onNeedJoining` command. For example:

```javascript
let channels = Libs.MembershipChecker.getChats();
Bot.sendMessage("Please join to our channels: " + channels);

```

Command `/onJoining`:

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

You can also perform manual check if you need button:

```javascript
// for all chats and channels:
// this method perform checking without delay
// but not more often than once every 2 seconds
Libs.MembershipChecker.check()
```
