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



### Get methods

You can call Api get methods. Need pass `on_result` key. 

For example get all user's profile photos:

#### Command `/get`

```javascript
Api.getUserProfilePhotos({
    user_id: user.telegramid,
    
    // this command will be executed after getting photos
    on_result: "onGetProfilePhotos"
});
```

#### 

#### Command `onGetProfilePhotos`

```javascript
// you can inspect result:
// Bot.inspect(options) 

if(!options.ok){
   return Bot.sendMessage("Error!");
}

if(options.result.total_count==0){
   return Bot.sendMessage("You have no photos in profile")
}

let photos = options.result.photos;
for(let i in photos){
   Api.sendPhoto( { photo: photos[i][0].file_id } );
}

```





