# MembershipChecker

This library is used to verify user membership in other channels and chats.

{% hint style="success" %}
It is recommended to use this library, since it allows you to make the bot work faster
{% endhint %}

## Initial setup

Install Library and create `/setup` command: 

```javascript
Libs.MembershipChecker.setup()
```

Then go to App > Bot > Admin Panels anf fill options:

![](<../.gitbook/assets/image (68).png>)

You need create `/onNeedJoining` command. For example:

```javascript
Bot.sendMessage("Please join to our channel: @MyChannel")
```

and `/onJoining`:

```javascript
Bot.sendMessage("Thank you")
```

Thats all.

## Additional

If membership is required to use the bot you can use [before all](https://help.bots.business/scenarios-and-bjs/always-running-commands#beforeall-and-afterall-commands) command: `@`

```javascript
// for all chats and channels:
isMember = Libs.MembershipChecker.isMember()

// for one chat / channel:
// isMember = Libs.MembershipChecker.isMember("@chatName")

if(!isMember){
  return // return from execution
}

```

You can also perfom manuall check if you need button:

```javascript
// for all chats and channels:
Libs.MembershipChecker.check()
```
