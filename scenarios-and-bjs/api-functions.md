# Api functions

Api functions it all functions from [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

You can use it with BJS. 

### **Example 1.** Send audio to current chat

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

### **Example 2.** Send photo with inline keyboard

![](../.gitbook/assets/image%20%2824%29.png)

```javascript
// see all parameters in https://core.telegram.org/bots/api#sendphoto
Api.sendPhoto({
  photo: "https://cataas.com/cat", // it is picture!
  caption: "Test photo",

  reply_markup: { inline_keyboard: [
    // line 1
    [
      // open the link on button pressing
      { text: "button1", url: "http://example.com" },
      // run command /onButton2 on button pressing
      { text: "button2", callback_data: "/onButton2" }
    ],
    // line 2
    [
       // see all params in
       // https://core.telegram.org/bots/api#inlinekeyboardbutton
       { text: "button3", callback_data: "/onButton3" }
    ]
  ]}
});
```

## Get methods

You can call Api get methods \(and others methods too\). Need pass `on_result` key. 

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

## Error handling

It is possible to capture error with `on_error` param

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3",
  on_error: "/on_error"
});
```

In command `on_error`:

```javascript
Bot.sendMessage("We have error with sending audio");
Bot.inspect(options)
```

