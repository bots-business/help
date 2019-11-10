# Inline Bot

We have [Inline bots](https://core.telegram.org/bots/inline) in Telegram:

[![](https://core.telegram.org/file/811140995/1/I-wubuXAnzk/2e39739d0ac6bd5458)](https://core.telegram.org/file/811140995/1/I-wubuXAnzk/2e39739d0ac6bd5458)

> Beyond sending commands in private messages or groups, users can interact with your bot via [**inline queries**](https://core.telegram.org/bots/api#inline-mode). If inline queries are enabled, users can call your bot by typing its username and a query in the **text input field** in **any** chat. The query is sent to your bot in an update. This way, people can request content from your bot in **any** of their chats, groups, or channels without sending any messages at all.

{% hint style="warning" %}
To enable this option, send the `/setinline` command to [@BotFather](https://telegram.me/botfather) and provide the placeholder text that the user will see in the input field after typing your bot’s name.
{% endhint %}

{% hint style="success" %}
See example bot [BBHelpBot](https://t.me/bbhelpbot)
{% endhint %}

## Support with BJS - command /inlineQuery

Need create command `/inlineQuery`. Such a name is required.

```javascript
// result.query - it is query from inline searching
if(!request.query){ return }

results = [];
totalResult = 0;

// it is array of results.
// we have InlineQueryResultArticle
// core.telegram.org/bots/api#inlinequeryresultarticle
// another types: https://core.telegram.org/bots/api#inlinequeryresult

results.push({
  type: "article",
  id: totalResult,
  title: "Text for item",
  input_message_content:
     { "message_text": "This message will be in chat" }
})

Api.answerInlineQuery({
  // see another fields at:
  // core.telegram.org/bots/api#answerinlinequery
  inline_query_id: request.id,
  results: results,
  cache_time: 30000 // cache time in sec
})
```

