# BB Admin functions

## BBAdmin.attractUser

| Function                       | Description        |
| ------------------------------ | ------------------ |
| `BBAdmin.attractUser(options)` | Attract new user.  |

Bot owner attract new user with email "test@example.com" to Bots.Business. Email must be valid.&#x20;

* User will be received email with password and information for start.
* Bot owner can see new user in App -> Account -> Attracted Users List
* Bot owner have rewards for each attracted user with paid Plan

{% hint style="info" %}
This function work only for new users. For old users - no any attraction and no email.
{% endhint %}

```javascript
BBAdmin.attractUser(
  { email: 'test@example.com'}
)
```

Also it is possible pass owner\_id (If not defined - used bot owner ID)

```javascript
BBAdmin.attractUser(
  { 
    email: 'test@example.com',
    owner_id: user_id // 
  }
)
```

## BBAdmin.installBot

| Function                    | Description                |
| --------------------------- | -------------------------- |
| BBAdmin.installBot(options) | Install bot for other user |

You can install copy of yours exist bot for user with email.

{% hint style="info" %}
If user is new user - they will be attracted as yours referral.
{% endhint %}

{% hint style="warning" %}
You can install only **your** bot for other users
{% endhint %}

{% hint style="success" %}
Bot can be installed as [protected](https://help.bots.business/protected-bot) bot.
{% endhint %}

{% hint style="info" %}
You can pass bot properties for new bot.
{% endhint %}

```javascript
BBAdmin.installBot(
  { 
    // bot will be cloned to this email
    email: 'test@example.com',
    // see bot id in the app -> Bots -> Bot
    bot_id: 15025,
    
    // you can pass bot token if you want
    token: BOT_TOKEN,
        
    // bot can be installed as protected
    // as_protected: true,
    
    // you can pass properties to bot:
    // bot_properties: [
    //     { name: 'test',
    //       value:'hello world',
    //       type:'string' }
    // ]
  }
)
```

{% hint style="info" %}
You can also clone your own bot with this function or use BBAdmin.cloneBot
{% endhint %}

## BBAdmin.cloneBot

| Function                  | Description   |
| ------------------------- | ------------- |
| BBAdmin.cloneBot(options) | Clone own bot |

You can create copy of yours exist bot

{% hint style="success" %}
Bot can be cloned as [protected](https://help.bots.business/protected-bot) bot.
{% endhint %}

{% hint style="info" %}
You can pass bot properties for new bot.
{% endhint %}

```javascript
BBAdmin.cloneBot(
  { 
    // see bot id in the app -> Bots -> Bot
    bot_id: 15025,
    
    // you can pass bot token if you want
    token: BOT_TOKEN,
    
    // run the bot immediately after cloning
    run_now: true,
    
    // bot can be installed as protected
    // as_protected: true,
    // you can pass properties to bot:
    // bot_properties: [
    //     { name: 'test',
    //       value:'hello world',
    //       type:'string' }
    // ]
  }
)
```

