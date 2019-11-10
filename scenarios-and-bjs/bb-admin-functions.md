# BB Admin functions

## BBAdmin.attractUser

| Function | Description |
| :--- | :--- |
| `BBAdmin.attractUser(options)` | Attract new user.  |

Bot owner attract new user with email "test@example.com" to Bots.Business. Email must be valid. 

* User will be received email with password and information for start.
* Bot owner can see new user in App -&gt; Account -&gt; Attracted Users List
* Bot owner have rewards for each attracted user with paid Plan

{% hint style="info" %}
This function work only for new users. For old users - no any attraction and no email.
{% endhint %}

```javascript
BBAdmin.attractUser(
  { email: 'test@example.com'}
)
```

Also it is possible pass owner\_id \(If not defined - used bot owner ID\)

```javascript
BBAdmin.attractUser(
  { 
    email: 'test@example.com',
    owner_id: user_id // 
  }
)
```

## BBAdmin.installBot

| Function | Description |
| :--- | :--- |
| BBAdmin.installBot\(options\) | Install bot for other user |

You can install copy of yours exist bot for user with email.

{% hint style="info" %}
If user is new user - they will be attracted as yours referral.
{% endhint %}

{% hint style="warning" %}
You can install only your bot for other users
{% endhint %}

{% hint style="info" %}
You can pass bot properties for new bot
{% endhint %}

```javascript
BBAdmin.installBot(
  { email: 'test@example.com',
    bot_id: 15025, // see bot id in the app -> Bots -> Bot
    // you can pass properties to bot:
    // bot_properties: [{ name: 'test', value:'hello world', type:'string' }]
  }
)
```

{% hint style="info" %}
You can clone your own bot with this function
{% endhint %}

