# BJS coding. How to..

## Q: What it is "BJS"?
It is Bot JavaScript. It's an ordinary Java with some inserts.

## Q: I do not know the JavaScript. What should I do?
Usually do not need something complicated for developing bots.
You can read a couple of articles about JS:
https://www.w3schools.com/js/js_syntax.asp or https://en.wikipedia.org/wiki/JavaScript_syntax

## Q: How can I get two answers from one command?
You can use bot answer and `Bot.sendMessage("ANY message").`

or use two functions:
BJS code:
```js
   Bot.sendMessage("ANY message");
   Bot.sendMessage("Other any message");
```

## Q: I have Markdown warning for message in Errors. What it is?
Telegram have markdown for text formating.

samples:
```
\n - new line (multi-line text also allowed)
*bold text*
_ italics _
[text](URL)
`inline fixed-width code`
```text
pre-formatted fixed-width code block

```

So if you have incorrect markdown - you have this warning.
Sample text with incorrect markdown:
```
  bot_name
  price 1*
  "user`s" - wrong. Use "user's"!
```

## Q: How I can use inline keyboard?
Please see [demo bot](https://telegram.me/DemoInlineKeyboardBot). It avaible in the Store.

BJS code:
```js
var buttons = [
    {title: "Go to Google", url: "https://google.com"},
    {title: "Call command for Button1", command: "/touch Button1" },
    {title: "Call command for Button2", command: "/touch Button2" }
];

Bot.sendInlineKeyboard(buttons, "Please make a choice. After that, another command `/touch` will be started with parameters");
```

`buttons` - it is array. It contains buttons. Each button is object with `title` (required), `url` or `command`.

Button must have `url` or `command`.
`url` - any link.

`command` - this command will be executed after button pressing. Can contain parameters through a space. `Command` can not be more then 64 bytes.

## Q: How to bot can send reply for users message
You need pass options parameter with `is_reply`

```js
  Bot.sendMessage("It is reply message", {is_reply: true} );
```

For any message in chat use: `reply_to_message_id`

```js
  Bot.sendMessage("It is reply message", {reply_to_message_id: request.message_id } );
```


Also you can retry with keyboard or inline keyboard too
```js
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
```js
  Bot.sendMessage( inspect(request) );
```


## Q: I would like to create bjs for time limit! Example in a bot you can only use the command every 24hrs!
BJS code:
```js
var last_run_at = User.getProperty("last_run_at");
if(last_run_at){
   duration_in_hours = ((new Date) - last_run_at) / 1000/60/60;
}else{
   // It is the first time!
   duration_in_hours = 99;
}
if(duration_in_hours>=24){
   User.setProperty("last_run_at", (new Date), "datetime")
   Bot.sendMessage("You run command now!");
   // add your code
   ...
   
}else{
   Bot.sendMessage("Please return later. ")
}
```

## Q: How i can create bjs that if you click button it directs to open a link in web?
Unfortunately, this is not supported by the Telegram API.
But you can send link to chat:
answer:
```
[Open](http://example.com)
```

## Q: How i can create password access to bot?
You can use a group for commands. Then such commands can be started only by those users who are in this group. You can assign users to a group through BJS with password verification.

*Example*
Bot:
> password?

User:
> 12345

Bot:
> Welcome, member!

In this example, the user must enter the correct password. After that, the group `Members` is setted and user can execute all commands of this group. If the password is not correct, a error message is displayed.

**Bot:**
answer: `password?`
need_reply: `true`

BJS code:
```js
if(data.message=="12345"){
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
```js
   if(data.chat.chat_type=="private"){
      // track only private chats
   
      total_users = Bot.getProperty("total_users");
      if(!total_users){ total_users = 0 }
      Bot.setProperty("total_users", total_users+1, "integer");

      var propLastActiveName = "user" + String(total_users) + "_last_active_at";
      var propChatIdName =  "chat" + String(i) + "_id";

      Bot.setProperty(propLastActiveName, (new Date), "datetime");
      Bot.setProperty(propChatIdName, data.chat.chatid, "string");  
   }
```

**2. In *others commands* you need call `tracking` command**
Bjs code:
```js
   Bot.runCommand("tracking");
   // your any code here
```

> Please note. This code is needed in all commands of your bot.

**3. Automatically message command.**
Set auto retry time for it: 24 hours

BJS:
```js
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
You can use [Coinmarketcap API](https://coinmarketcap.com/api/). 
For example page https://api.coinmarketcap.com/v2/ticker/1/ have information about Bitcoin.
We need load it with BJS.

BJS:
```js
  ->(https://api.coinmarketcap.com/v2/ticker/1/)
  var result = JSON.parse(data.content);
  var BTC_USD_Price = result.data.quotes.USD.price;
  Bot.sendMessage("Current Bitcoin price: " + String(BTC_USD_Price) + " $");
```

## Q: Is it posible that bot button can have value? How to create like on the screenshot below:
![](https://i.imgur.com/6bA89pW.png)

You must update the keyboard every time the value changes. To do this, send the keyboard with the command. Most likely, this should be done in several commands.

BJS:
```js
var balance = User.getProperty("balance"); // or your code here
Bot.sendKeyboard(String(balance) + ",\nHelp, Contacts" );
```

**Important**
> Now you can not set command to this button!
