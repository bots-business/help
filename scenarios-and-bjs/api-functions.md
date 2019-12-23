# Api وظائف

Api وظائف كل شيء من وظائف [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

يمكنك استخدامه مع BJS.

 ### ** مثال 1. ** إرسال الصوت إلى الدردشة الحالية

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```

إرسال الصوت إلى الدردشة الأخرى:

```javascript
Api.sendAudio({
  chat_id: 5515411,
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
});

```



يمكنك تمرير المعلمات المسموح بها.  على سبيل المثال ل [sendAudio](https://core.telegram.org/bots/api#sendaudio) يمكن أن يكون العنوان و disable\_notification

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3"
  title: "test audio",
  disable_notification: true
});
```

### ** مثال 2. ** إرسال الصور مع لوحة المفاتيح المضمنة

![](../.gitbook/assets/image%20%2824%29.png)

```javascript
// رؤية جميع المعلمات في https://core.telegram.org/bots/api#sendphoto
Api.sendPhoto({
  photo: "https://cataas.com/cat", // it is picture!
  caption: "Test photo",

  reply_markup: { inline_keyboard: [
    // line 1
    [
      // فتح الرابط على زر الضغط
      { text: "زر 1", url: "http://example.com" },
      // run command /onButton2 on button pressing
      { text: "2زر", callback_data: "/onButton2" }
    ],
    // line 2
    [
       // رؤية جميع المعلمات في
       // https://core.telegram.org/bots/api#inlinekeyboardbutton
       { text: "button3", callback_data: "/onButton3" }
    ]
  ]}
});
```

## الحصول على الأساليب

يمكنك استدعاء أساليب الحصول على Api
\(and others methods too\).
تحتاج تمرير
`on_result` key. 

على سبيل المثال الحصول على جميع صور الملف الشخصي للمستخدم:

#### Command `/get`

```javascript
Api.getUserProfilePhotos({
    user_id: user.telegramid,
    
    // سيتم تنفيذ هذا الأمر بعد الحصول على الصور
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
   return Bot.sendMessage("ليس لديك صور في الملف الشخصي")
}

let photos = options.result.photos;
for(let i in photos){
   Api.sendPhoto( { photo: photos[i][0].file_id } );
}

```

## معالجة الأخطاء

 من الممكن التقاط الخطأ مع التوقف
`on_error`

```javascript
Api.sendAudio({
  audio: "https://www.bensound.org/bensound-music/bensound-funnysong.mp3",
  on_error: "/on_error"
});
```

In command `on_error`:

```javascript
Bot.sendMessage("لدينا خطأ في إرسال الصوت");
Bot.inspect(options)
```

