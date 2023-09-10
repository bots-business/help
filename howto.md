# How to...

## Q: What it is "BJS"?

It is Bot JavaScript. It's an ordinary Java with some inserts.

## Q: I do not know the JavaScript. What should I do?

Usually do not need something complicated for developing bots. You can read a couple of articles about JS: [https://www.w3schools.com/js/js\_syntax.asp](https://www.w3schools.com/js/js\_syntax.asp) or [https://en.wikipedia.org/wiki/JavaScript\_syntax](https://en.wikipedia.org/wiki/JavaScript\_syntax)

## Q: How can I get two answers from one command?

You can use bot answer and `Bot.sendMessage("ANY message").`

or use two functions: BJS code:

```javascript
   Bot.sendMessage("ANY message");
   Bot.sendMessage("Other any message");
```

## Q: I have Markdown warning for message in Errors. What it is?

Telegram have markdown for text formating.

samples:

````
\n - new line (multi-line text also allowed)
*bold text*
_ italics _
[text](URL)
`inline fixed-width code`
```text
pre-formatted fixed-width code block
````

So if you have incorrect markdown - you have this warning. Sample text with incorrect markdown:

```
  bot_name
  price 1*
  "user`s" - wrong. Use "user's"!
```

## Q: How I can use inline keyboard?

Please see [demo bot](https://telegram.me/DemoInlineKeyboardBot). It avaible in the Store.

BJS code:

```javascript
var buttons = [
    {title: "Go to Google", url: "https://google.com"},
    {title: "Call command for Button1", command: "/touch Button1" },
    {title: "Call command for Button2", command: "/touch Button2" }
];

Bot.sendInlineKeyboard(buttons, "Please make a choice. After that, another command `/touch` will be started with parameters");
```

`buttons` - it is array. It contains buttons. Each button is object with `title` (required), `url` or `command`.

Button must have `url` or `command`. `url` - any link.

`command` - this command will be executed after button pressing. Can contain parameters through a space. `Command` can not be more then 64 bytes.

## Q: How to bot can send reply for users message

You need pass options parameter with `is_reply`

```javascript
  Bot.sendMessage("It is reply message", {is_reply: true} );
```

For any message in chat use: `reply_to_message_id`

```javascript
  Bot.sendMessage("It is reply message", {reply_to_message_id: request.message_id } );
```

Also you can retry with keyboard or inline keyboard too

```javascript
  // retry for last message
  Bot.sendMessage("It is reply message", {is_reply: true } );
  // or for any message
  Bot.sendMessage("It is reply message", {reply_to_message_id: request.message_id } );

  Bot.sendInlineKeyboard(
      [ {title: "google", url: "http://google.com" }, {title: "other command", commnad: "/othercommand"} ],
      "Please make a choice.",
      {reply_to_message_id: request.message_id }
 )
```

You can retry for any message from chat.

## Q: I do not undestand variables: `request`, `user`, `chat`, and etc.

You can see it with `inspect` function:

```javascript
  Bot.sendMessage( inspect(request) );
  // or 
  Bot.inspect(request)   // if have issue with markdown
```

## Q: I would like to create bjs for time limit! Example in a bot you can use the command only every 24hrs!

BJS code:

```javascript

function canRun(){
  var last_run_at = User.getProperty("last_run_at");
  if(!last_run_at){ return true }
  
  var minutes = (Date.now() - last_run_at) /1000/60;
  
  var minutes_in_day = 24*60
  
  if(minutes < minutes_in_day){
   Bot.sendMessage("Please return later. ")
   return
  }
  return true;
}

if(!canRun()){ return }
User.setProperty("last_run_at", Date.now(), "integer");

// your code here:
// ...
```

## Q: How i can create bjs that if you click button it directs to open a link in web?

Unfortunately, this is not supported by the Telegram API. But you can send link to chat: answer:

```
[Open](http://example.com)
```

## Q: How i can create password access to bot?

You can use a group for commands. Then such commands can be started only by those users who are in this group. You can assign users to a group through BJS with password verification.

_Example_ Bot:

> password?

User:

> 12345

Bot:

> Welcome, member!

In this example, the user must enter the correct password. After that, the group `Members` is setted and user can execute all commands of this group. If the password is not correct, a error message is displayed.

**Bot:** answer: `password?` need\_reply: `true`

BJS code:

```javascript
if(message=="12345"){
  User.addToGroup('Members');
  Bot.sendMessage("Welcome, member!");
}else{
  Bot.sendMessage("Password incorrect");
}
```

## Q: Do you know BJS code that bot will automatically message user if they don't do any activity in the bot in a given time?

**1. You can store Last Active time** for user in bot's property:

command `tracking`

> this is an invisible command for users. It is run from other commands only

BJS code:

```javascript
   if(chat.chat_type=="private"){
      // track only private chats

      total_users = Bot.getProperty("total_users");
      if(!total_users){ total_users = 0 }
      Bot.setProperty("total_users", total_users+1, "integer");

      var propLastActiveName = "user" + String(total_users) + "_last_active_at";
      var propChatIdName =  "chat" + String(i) + "_id";

      Bot.setProperty(propLastActiveName, (new Date), "datetime");
      Bot.setProperty(propChatIdName, chat.chatid, "string");  
   }
```

**2. In **_**others commands**_** you need call** `tracking` **command** Bjs code:

```javascript
   Bot.runCommand("tracking");
   // your any code here
```

> Please note. This code is needed in all commands of your bot.

**3. Automatically message command.** Set auto retry time for it: 24 hours

BJS:

```javascript
   total_users = Bot.getProperty("total_users");
   for(var i=0; i<total_users; i++){
      var propLastActiveName = "user" + String(i) + "_last_active_at";
      var last_active_at = Bot.getProperty(propLastActiveName);
      var duration = (new Date) - last_active_at;
      var duration_in_minutes = duration / 1000 / 60
      if(duration_in_minutes>60*24){
         // not active more than 24 hours
         var propChatIdName = "chat" + String(i) + "_id";
         var chatId = Bot.getProperty(propChatIdName);
         Bot.sendMessageToChatWithId(chatId, "Hello! How are you?");
      }
   }
```

## Q: Is it possible to put some BJS code in our bot that notify user for cryptocurrency price?

You can use [CurrencyQuote](libs/currencyquote.md) Lib



## Q: Is it posible that bot button can have value? How to create like on the screenshot below:

![](https://i.imgur.com/6bA89pW.png)

You must update the keyboard every time the value changes. To do this, send the keyboard with the command. Most likely, this should be done in several commands.

**BJS**:

```javascript
var balance = User.getProperty("balance"); // or your code here
Bot.sendKeyboard(String(balance) + ",\nHelp, Contacts" );
```

{% hint style="info" %}
**Use ResLib for any resources**
{% endhint %}

#### Command `⚡Balance:`

You need create command "⚡Balance:" (without space) or `"⚡"` if you have space beetween "⚡" and "Balance"

**So after button pressing:**

* text "⚡Balance: X BTC⚡" will be sended to chat
* command "Balance:" will be executed
* params "X BTC⚡" will be passed to BJS

## Q: How to set the bot which it result to, when one of the telegram group members click the bot command in telegram bot, the message from the bot only seen by the clickers it self & unseen by another members.

Bot can send message to this user in private. So user need to start this bot in private chat in first.

Then on group chat:

```javascript
Bot.sendMessageToChatWithId(user.telegramid, "BOT ANSWER")
```

Also it is possible show alert message for user in group chat with [answerCallbackQuery](https://core.telegram.org/bots/api#answercallbackquery) after inline button pressing.



## Q: How to get location from user?

User must attach his location to chat.

Need command with "Wait for answer" option (or you can use Master command and catch location)

BJS:

```javascript
// you can inspect all data
// Bot.inspect(request);

let location = request.location
if(!location){
   Bot.sendMessage("Please send location");
   return
}

Bot.sendMessage(
   "Your location is:\n longitude " +
       location.longitude +
       "\n latitude: " +
       location.latitude
 )
```

## Q: I have command with wait fo reply. How to cancel Wait for reply?

Example - command `/askName` have wait for reply. Need to cancel it.

Add keyboard to this command: keyboard "Cancel" (also can be "Back")

BJS:

```javascript
// exit on Cancel or Back button
if((message=="Cancel")||(message=="Back")){
  return // exit
}

// get name here
name = message;
Bot.sendMessage("Hello, " + name);

```



## Q: How to show alert on Inline button pressing?

BJS:

```javascript
Api.answerCallbackQuery({
  callback_query_id: request.id,
  text: "My alert",
  show_alert: true // or false - for alert on top
})
```



## Q: How to check if a user joins a channel?

{% hint style="success" %}
Strongly recommended use [MembershipChecker](libs/mcl.md) Lib for this
{% endhint %}

Command: `/isJoined`

```javascript
chanell = "@MyChanell"

Api.getChatMember({
  chat_id: chanell,
  user_id: user.telegramid,
  on_result :"/onCheckJoin"
})
```

Command `/onCheckJoin`

```javascript
let status = options.result.status;

var isJoined = (
   (status == "member")||
   (status == "administrator")||
   (status == "creator")
)

if(isJoined){
   Bot.sendMessage("You are chanell member!");
}else{
   Bot.sendMessage("You are NOT chanell member!");
}
```
