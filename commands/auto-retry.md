# Auto Retry (AR)

Command can be run periodically. For example:

* bot send [message "Hello" ](https://help.bots.business/store/welcome-bot#good-morning-every-day)every 24 hours
* bot download web page every 1 hour and parse it. See our [PlayMarketNewsBot](https://telegram.me/PlayMarketNewsBot)

{% hint style="danger" %}
Auto Retry spent 1 iteration on each run. Thus, if you put AR for once a minute (60 secs), it will be 1440 iterations per day.
{% endhint %}

So it is need set auto retry time:

* for 10 minutes: 60\*10 = **600** secs
* for 1 hour: 60\*60 = **3600** secs
* for 24 hours: 60\*60\*24 = **86400** secs.
* for 1 year: 86400 \* 365 = **24966000** secs

#### Modify Auto Retry in app on command editing:

<figure><img src="../.gitbook/assets/изображение (6).png" alt=""><figcaption></figcaption></figure>



{% hint style="warning" %}
Auto retry works only with [BJS](https://help.bots.business/scenarios-and-bjs). There are now [Variables](https://help.bots.business/scenarios-and-bjs/variables): chat, user, request.
{% endhint %}

### Handled only on BJS!

Because Auto Retry initialized by automatic there are no current chat, user and request.&#x20;

So we can not use:

```javascript
Bot.sendMessage("Hello");  //not works with Auto Retry
```

Why? Bot do not know chat for sending message! No current chat.

So it is need define chat:

```javascript
Bot.sendMessage({text: "Hello", chat_id: YOUR_CHAT_ID});
```

#### How I can know chat id?

Create simple command (without Aoto Retry): `/chat` with BJS:

```javascript
Bot.sendMessage(chat.chatid);
```

And run it on that chat where you need Auto Retry later. This command return YOUR\_CHAT\_ID

Fill it in previous command with Auto Retry.



{% hint style="info" %}
You can use Bot.getProperty and Bot.setProperty. So you can save chat\_id in one command and then get it on Auto Retry command&#x20;
{% endhint %}

{% hint style="warning" %}
You can not use User.getProperty and User.setProperty.&#x20;

No user on Auto Retry!
{% endhint %}







