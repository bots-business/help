# Api functions

Api functions it all functions from [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

You can use it with BJS. 

For example: send audio to current chat:

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```

send audio to other chat: 

```javascript
Api.sendAudio({
  chat_id: 5515411,
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```



You can pass allowed parameters. For example for [sendAudio](https://core.telegram.org/bots/api#sendaudio) it can be title and disable\_notification

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
  title: "test audio",
  disable_notification: true
});
```



{% hint style="info" %}
You can use all methods now except GET methods
{% endhint %}

{% hint style="warning" %}
Get methods in progress
{% endhint %}









