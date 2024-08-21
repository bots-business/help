# Webhooks lib

Integration with external services can be possible with webhooks notifications. This lib generate url for webhooks.

## Example bot

See [example bot](https://t.me/BBWebhookBot)

{% hint style="info" %}
From [wikipedia](https://en.wikipedia.org/wiki/Webhook): a **webhook** is a method of augmenting or altering the behavior of bot, with custom [callbacks](https://en.wikipedia.org/wiki/Callback\_\(computer\_programming\)).

These callbacks may be maintained, modified, and managed by third-party users and developers who may not necessarily be affiliated with the originating website or application.

&#x20;The term "webhook" was coined by Jeff Lindsay in 2007 from the computer programming term [hook](https://en.wikipedia.org/wiki/Hooking).
{% endhint %}

Webhooks is more simple way for integration. Other libs also use webhooks notifications already: CoinPayments, FreeKassa.

{% hint style="success" %}
Webhook link have public\_user\_token - it is public secret.

User can't modify user\_id, command because it is protected with public\_user\_token.
{% endhint %}

## Get Webhook Url

```javascript
// user's webhook
let webhookUrl = Libs.Webhooks.getUrlFor({
  // this command will be runned on webhook
  command: "/onWebhook",
  // this text will be passed to command
  content: "Did you see the cat?",
  // execute for this (current) user
  user_id: user.id,
  // redirect to page with cat after calling webhook
  // you need remove this for external service
  redirect_to: "https://cataas.com/cat"
})

Bot.inspect(webhookUrl);
```

This code will generate Webhook url.&#x20;

After loading page via this url:

* web page with cat will be loaded (Thank for cat to [https://cataas.com](https://cataas.com/cat))
* command `/onWebhook` will be execute on Bot for user with user.id
* content "Did you see the cat?" will be passed for command `/onWebhook`

### &#x20;

## Receive webhook for user

As a rule, the webhook URL must be set from the admin panel on the external service. So we can not set it for just one user:&#x20;

```javascript
// global bot webhook
let webhookUrl = Libs.Webhooks.getUrlFor({
  // this command will be runned on webhook
  command: "/onWebhook",
  user_id: user.id
})
```

{% hint style="warning" %}
Webhooks can be with GET and POST methods only. All passed data contains on content variable
{% endhint %}

On command `/onWebhook` we can get posted content from external service

```javascript
// for user's webhook
Bot.sendMessage(inspect(content))
// also you can read data with Bot.getProperty - you need store it before
```

As a rule, external service must pass useful data on webhook. For example info about payments: order\_id, user\_id. Use it!



## Receive global webhook for bot

On command `/onWebhook` for bot's webhook we do not have user:

```javascript
// for bot's webhook

// this is not worked - because no current user on bot's webhook
// Bot.sendMessage(inspect(content))

// make any not specific user code
// ...

// We can pass user_id in content (it is depend from external service)
Bot.run({
   command: "/userCommand",
   user_id: JSON.parse(content).user_id
})

// in /userCommand we can now use Bot.sendMessage function

```

## Call options

You can use options on BJS on webhook request

`/onWebhook` command:

```javascript
Bot.sendMessage(
  JSON.stringify(options)
);
```

| Key               | Value                                                                  |
| ----------------- | ---------------------------------------------------------------------- |
| `options.url`     | webhook url                                                            |
| `options.method`  | request method ("GET" or "POST")                                       |
| `options.params`  | request params                                                         |
| `options.headers` | headers for this request like Ip,  User-Agent, Accept-Language and etc |
| `options.ip`      | client IP                                                              |



## Webhook response

### 1. Content response with bot answer

```javascript
// call user's webhook
// first message sending appears on bot
Bot.sendMessage("This answer will be in bot");

//...
// last message sending
// will be on bot and on web page
// you can open this web page on your browser via webhook url

Bot.sendMessage("Hello in browser!");

// in web page you will have:
// { answer: "Hello in browser!" }
```

### 2. Content response without bot answer

You can use [WebApp](../bjs/web-app.md) render - it is not produce message from bot.

```javascript
// render this command in web
WebApp.render({ content: "Hello from bot " + bot.name });
```

### Code response

Webhook response can be:

* 200 - BJS is runned, no errors
* 503 - we have errors in BJS

you can throw error in BJS:

`throw new Error("Error on webhook")`





## Possible issues

In the case of a large number of requests from an external web service, such a web service may be subject to ban filters.

Please provide an external IP address and we will add it to the White List.

