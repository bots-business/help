# GoogleApp

Use this lib to connect BJS with [Google App Script](https://developers.google.com/apps-script)

![](../.gitbook/assets/image%20%2876%29.png)

## Getting started

1. Go to [https://script.google.com](https://script.google.com) and create new project by button:

![](../.gitbook/assets/image%20%2875%29.png)

2. Paste the script from [above](https://gist.github.com/bots-business/0635dfc5ee3b28328e93239a607d680c) into the script code editor and hit _Save._

![](../.gitbook/assets/image%20%2874%29.png)

**3.**  From the _Publish_ menu, select _Deploy as web app…_  
  
4**.** Choose to execute the app as **yourself**, and allow _**Anyone**, even **anonymous**_ to execute the script. \(Note, depending on your Google Apps instance, this option may not be available. You will need to contact your Google Apps administrator, or else use a Gmail account.\) Now click _Deploy_. You may be asked to review permissions now. **Project version** - always "New".

![](../.gitbook/assets/image%20%2872%29.png)

**5.** The URL that you get will be the webhook that you need use in this Lib. You can test this webhook in your browser first by pasting it. Note that depending on your Google Apps instance, you may need to adjust the URL to make it work. 

6. Install GoogleAppLib and WebhooksLib to bot

7. Create setup command:

**`/setup`**

```javascript
// replace with your URL, obtained in step 4
Libs.GoogleApp.setUrl("https://script.google.com/macros/*******");
```

## Using

### Use any Google App script in BJS now

```javascript
function GACode(){
   // Google App Script code here
   // ...
}

// BJS
Libs.GoogleApp.run({
  code: GACode, // Function with Google App code
  onRun: "onRun", // Optional. This command will be executed after run
  email: "my@email.com" // Optional. Email for errors,
  // debug: true // For debug. Default is false
});
```

Example for command `/task`

```javascript
function GACode(){
  // translation from English to France
  var trans = LanguageApp.translate(params, 'en', 'fr');

  // make Google Calendar event
  var event = CalendarApp.getDefaultCalendar()
    .createEventFromDescription('Lunch with ' + user.first_name + ', Friday at 1PM');
  
  // send Email
  MailApp.sendEmail({
    to: "help@example.com",
    subject: "hello from bot " + bot.name,
    htmlBody: "<h1>Hello!</h1>How are you?<br>" +
        "We have message to bot: " + message
  });

  // ...
  // Use all power of Google App Script!
  
  // return result as JSON
  return { event: event, trans: trans }
}

Libs.GoogleApp.run({
  func: GACode,
  onRun: "onRun",
  // email: "test@example.com" // email for errors
  // debug: true // default false
});
```

command `onRun`

```javascript
Bot.sendMessage(inspect(options))
```

### Permissions

{% hint style="warning" %}
You need set permissions for Google App Script
{% endhint %}

Run `Libs.GoogleAppLib.run`in first time. You can have like such error:

> Error on Google App script: "Exception"
>
> "The script does not have permission to perform that action. Required permissions: \([https://www.googleapis.com/auth/calendar](https://www.googleapis.com/auth/calendar) \|\| [https://www.googleapis.com/auth/calendar.readonly](https://www.googleapis.com/auth/calendar.readonly) \|\| [https://www.google.com/calendar/feeds](https://www.google.com/calendar/feeds)\)"

If you have such error:

* go to Google App Script Editor \(See step 2\)
* select "Debug" function on Tab
* press "Run" button:

![](../.gitbook/assets/image%20%2873%29.png)

After this grant all needed permissions for your script

{% hint style="success" %}
You can use "debug" function anytime for debugging
{% endhint %}

