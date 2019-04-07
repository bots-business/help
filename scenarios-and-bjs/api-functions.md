# Функции Api

Функции Api это все функции с [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

Вы можете использовать их в BJS. 

Например: отправить аудио в текущий чат:

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```

отправить аудио в другой чат: 

```javascript
Api.sendAudio({
  chat_id: 5515411,
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```



Вы можете передать разрешенные параметры. Например для [sendAudio](https://core.telegram.org/bots/api#sendaudio) Это могут быть заголовок и disable\_notification(отключение уведомления)

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
  title: "Тестовое аудио",
  disable_notification: true
});
```



{% hint style="info" %}
Вы можете использовать все методы, кроме методов GET
{% endhint %}

{% hint style="warning" %}
Get методы в прогрессе
{% endhint %}









