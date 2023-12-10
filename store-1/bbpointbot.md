# BB Point Bot

BB Point bot helps develop the community. User can echange BB Points to Extra Iterations.

Also it is possible:

* Accept BB Points 💎 in your bot from users
* Transfer BB Points 💎 from your bot to users

{% hint style="success" %}
This functionality is built with [WebhookLib](../libs/webhooks-lib.md). You can do some pretty cool stuff in your bots to share data too.
{% endhint %}

## Accept BB Points in your bot

![](https://telegra.ph/file/31b497c82e26a1dc2d8d3.png)

We can accept BB Points 💎 in any bot now.

See example in [@BBWebhookBot](https://t.me/BBWebhookBot)

### How to make?

See example for /sample3 in [@BBWebhookBot](https://t.me/BBWebhookBot)

#### Step 1

Install Webhook Lib in your bot and make webhook url:

```javascript
// Generate webhook link for BB Point Bot
let url = Libs.Webhooks.getUrlFor({
 command: "onBBPointIncome"
})

Bot.sendMessage(
 "Set this url in [@BBPointBot](https://t.me/BBPointBot?start=link) " +
 "bot for notification." +
 "\n\nCommand [@BBPointBot > /link](https://t.me/BBPointBot?start=link) "
)

// send url without markup
Api.sendMessage({ text: url });
```



#### Step 2 <a href="#step-2" id="step-2"></a>

Go to [@BBPointBot](https://t.me/BBPointBot) - **/link** and paste link from step1

You will get such link for request:

https://t.me/BBPointBot?start=req15-**1**-points-to-519829299

> You can change **bb point** amount in url part: **-XXX-points**

> You can change **user.id** after part: -**to-user-XXX**



## Transfer BB Points 💎 from your bot to users

#### Step 1

Generate your personal secret webhook url in [@BBPointBot](https://t.me/BBPointBot) by command: `/getTransferUrl`

Set **Secret (**any words or symbols - whatever you want**)** in [@BBPointBot](https://t.me/BBPointBot) by command: `/setTransferSecret`

{% hint style="warning" %}
Personal secret webhook url with Secret - is safe information.&#x20;

You can reset the Secret at any time via command `/setTransferSecret`&#x20;
{% endhint %}



#### Step 2

In your bot command create new command `/makeTransfer`:

```javascript
// Danger! User can run this command
// You need add logic for secure
// if(your logic){ return }

// Just generate webhook url for current user
let webhookUrl = Libs.Webhooks.getUrlFor({
  command: "onTransfer",
  user_id: user.id
})

Bot.sendMessage("Transfer in progress")

// make transfer request to BB Point bot
HTTP.post( {
    url: "http://Your Personal secret webhook url from step 1",
         
    body: {
       // BB Points amount
       amount: 3,
       // transfer BB Points for current user
       to_tg_id: user.telegramid,
       // note for @bbpoints channel
       note: "#testTransfer by " + bot.name,
       webhookUrl: webhookUrl,
       secret: "your Secret" // from step 1
    }
} )

```

{% hint style="danger" %}
This command transfer BB Points without any conditions.

You must add some conditions. You are not going to send BB Points without any reason?&#x20;
{% endhint %}

**Step 3**

In your bot command create new command `onTransfer`:

```javascript
var json = JSON.parse(content);

// You can inspect all passed data:
// Bot.inspect(json)

if(json.error){
  Bot.sendMessage("Error: " + json.error.title);
  Bot.sendMessage("Code: " + json.error.code);
  // error codes:
  // 1 - You do not have BB Points for transfer
  return
}

// BB Points transferred to current user
let admin_bb_points = json.owner.bb_points - json.amount;

Bot.sendMessage(
    "BB Points transferred:\n" +
    json.amount + "💎 BB Points to tg id: " + user.telegramid + 

    "\n\nAdmin: @" + json.owner.username + 
       "\n have now: " + String(admin_bb_points) + "💎 BB Points"
)


```

