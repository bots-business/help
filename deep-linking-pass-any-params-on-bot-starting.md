# Deep Linking - pass any params on Bot starting

You can pass any params on bot starting to BJS with link:

[http://t.me/BOT\_NAME?start=PARAMS](http://t.me/BOT_NAME?start=PARAMS)

In BJS command `/start`:

```javascript
Bot.sendMessage(params);
```

For example:

[http://t.me/botsbusinessadminbot?start=API\_TOKEN](http://t.me/botsbusinessadminbot?start=API_TOKEN)

We pass user's API Token to bot.

```javascript
let api_token = params;
// do anything what you want with this params
Bot.sendMessage("Your api token is: " + api_token)
```

