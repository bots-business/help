# GoogleApp

Use this lib to connect BJS with [Google App Script](https://developers.google.com/apps-script)

![](<../.gitbook/assets/image (76).png>)

## Getting started

1\. Go to [https://script.google.com](https://script.google.com) and create new project by button:

![](<../.gitbook/assets/image (77).png>)

2\. Paste the script from [above](https://github.com/bots-business/store-libs/blob/master/GoogleAppSync.gs) into the script code editor and hit _Save._

![](<../.gitbook/assets/image (95) (1) (1).png>)

You will need to contact your Google Apps administrator, or else use a Gmail account.)

Now click _Deploy_. You may be asked to review permissions now. **Project version** - always "New".

![](<../.gitbook/assets/image (75).png>)

**5.** The URL that you get will be the webhook that you need use in this Lib. You can test this webhook in your browser first by pasting it. Note that depending on your Google Apps instance, you may need to adjust the URL to make it work.&#x20;

6\. Install _GoogleAppLib_ and _WebhooksLib_ to bot

7\. Create setup command:

**`/setup`**

```javascript
// replace with your URL, obtained in step 4
Libs.GoogleApp.setUrl("https://script.google.com/macros/*******");
```

## Using

Use any Google App script in BJS now

```javascript
function GACode(){
   // Google App Script code here
   // Please note: this function is runs on GA not BB
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

{% hint style="warning" %}
You can turn on debug flag with true

Then you can [debug](googleapp.md#debugging) code
{% endhint %}

command `onRun`

```javascript
Bot.sendMessage(inspect(options))
```

{% hint style="warning" %}
**GACode** - it is isolated function with Google App code. It can not have BJS code like `Bot.sendMessage` and etc. Only GA code!

**But you can** use [variables](../bjs/variables.md) and pass data with [options](../bjs/bot-functions.md#bot-run-options) for Bot.run method.

```javascript
let myVar;

function GACode(){
   // Google App Script code here
   // Please note: this function is runs on GA not BB
   // ...
   
   myVar = 5; // Error! myVar is not defined in GA only in BB side
   
   // this will be works:
   let myVar;
   myVar = 5;
   
   Bot.sendMessage("ok")  // Error! It is BJS not GA code!
   
   let botName = bot.name; // Will be worked
   let myData = options.myData; // Will be worked if you pass 
   // options.myData with Bot.run(
   //    { command: "/thisCmd", options: { myData: "test" } })
}
```
{% endhint %}



## Permissions

{% hint style="warning" %}
You need set permissions for Google App Script
{% endhint %}

Run `Libs.GoogleAppLib.run`in first time. You can have like such error:

> Error on Google App script: "Exception"
>
> "The script does not have permission to perform that action. Required permissions: ([https://www.googleapis.com/auth/calendar](https://www.googleapis.com/auth/calendar) || [https://www.googleapis.com/auth/calendar.readonly](https://www.googleapis.com/auth/calendar.readonly) || [https://www.google.com/calendar/feeds](https://www.google.com/calendar/feeds))"

If you have such error you need set access rights.

### Granting access rights via manifest file

Full help available [here](https://developers.google.com/apps-script/concepts/scopes#setting\_explicit\_scopes). From that help:



1. Open the script project.
2. At the left, click **Project Settings** settings.
3. Select the **Show "appsscript.json" manifest file in editor** checkbox:

![](<../.gitbook/assets/image (90).png>)



At the left, click **Editor** code.

At the left, click the `appsscript.json` file.

Locate the top-level field labeled `oauthScopes`. If it's not present, you can add it.

The `oauthScopes` field specifies an array of strings. To set the scopes your project uses, replace the contents of this array with the scopes you want it to use. For example:

```
{
  "timeZone": "Asia/Tokyo",
  "dependencies": {
  },
  "webapp": {
    "access": "ANYONE_ANONYMOUS",
    "executeAs": "USER_DEPLOYING"
  },
  "exceptionLogging": "STACKDRIVER",
  "oauthScopes": ["https://www.googleapis.com/auth/script.send_mail",
                  "https://www.googleapis.com/auth/script.external_request",
                  "https://www.googleapis.com/auth/spreadsheets"],
  "runtimeVersion": "V8"
}
```

1. [https://www.googleapis.com/auth/script.send\_mail](https://www.googleapis.com/auth/script.send\_mail)", "[https://www.googleapis.com/auth/script.external\_request](https://www.googleapis.com/auth/script.external\_request) - is mandatory scope
2. Save the manifest file using **Ctrl+S** or the Save file icon in the menu bar.
3. Publish your app again (see [step 3](googleapp.md#getting-started))

![](<../.gitbook/assets/image (82).png>)



## Debugging

Run `Libs.GoogleAppLib.run`in first time. Then:

* go to Google App Script Editor (See [step 2](googleapp.md#getting-started))
* select "debug" function on Tab
* press "Debug" button:

![](<../.gitbook/assets/image (93).png>)

Google app is runs. Bot will sent execution result to you. Also you can receive email with error description.

{% hint style="success" %}
You can use "debug" function anytime for debugging
{% endhint %}



Also you can open web app by url (see [step 5](googleapp.md#getting-started)) in incognito mode. And look for any errors. For example we have permission error here:

![](<../.gitbook/assets/image (94) (1).png>)





## Links

[Google App script](https://developers.google.com/apps-script) home page

Google App Script [examples](https://github.com/gsuitedevs/apps-script-samples) - good examples for inspiration

Stack Overflow [answers](http://stackoverflow.com/questions/tagged/google-apps-script) - ask your questions on SO

[Videos](https://developers.google.com/apps-script/guides/videos) -  Check out the Apps Script videos on YouTube

[Reference](https://developers.google.com/apps-script/reference) - The reference documentation provided in this section describes the various Apps Script services and the Apps Script manifest file structure.
