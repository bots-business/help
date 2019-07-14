# Welcome bot

This bot greeting all new members in chat and say "Good morning" every day.

### Good morning every day

We use [Auto Retry](https://help.bots.business/commands/auto-retry) for this. Every 50 minutes we check time. If current hour is 6 AM - bot send greeting message to all chat.

{% hint style="info" %}
Auto retry run periodically.

We have 50 minutes - so we iterate every hour without repeating.
{% endhint %}

Set 3000 to Auto Retry:

![](../.gitbook/assets/image%20%2810%29.png)

#### Users can not run this command.

Bot send message to ALL chats. If user run this command at 6 AM - bot send message too! 

We need check for auto retry only. Auto Retry do not have chat variable. Execution breaks if chat exist.

```javascript
// can be runned with Auto Retry only!
if(chat){ return }
```

Then other code:

```javascript
let time = new Date()
let hours = time.getHours();
let minutes = time.getMinutes();

curTime = "Time: " + hours + ":" + minutes + " GMT-0";
msg = "";

if(hours==6){
  msg = "Good morning!\n" + curTime;
}

Bot.sendMessageToAllChats(msg);
```



### Greeting all new member

We use Master commad "\*" for this. it capture all messages.

Request can have information about new members:

```javascript
let new_members = request.new_chat_members;
```

We need build greetings for all users:

```javascript
if(new_members.length > 0){
   for(var i=0; i<new_members.length; i++){
      msg = msg + comma + getNameFor(new_members[i])
      comma = ", ";
   }
   Bot.sendMessage(msg);
}
```

User can have username or first name. Or have not:

```javascript
function getNameFor(member){
   let haveAnyNames = member.username&&member.first_name;
   if(!haveAnyNames){ return ""}

   return member.username ? ("@" + member.username) : member.first_name
}
```



#### All code: 

```javascript
let new_members = request.new_chat_members;
let msg = "Hello, ";
let comma = "";

function getNameFor(member){
   let haveAnyNames = member.username&&member.first_name;
   if(!haveAnyNames){ return ""}

   return member.username ? ("@" + member.username) : member.first_name
}

if(new_members.length > 0){
   for(var i=0; i<new_members.length; i++){
      msg = msg + comma + getNameFor(new_members[i])
      comma = ", ";
   }
   Bot.sendMessage(msg);
}

if(request.left_chat_member){
  Bot.sendMessage(
    "Goodbye, " + getNameFor(request.left_chat_member)
  );
}

```

![](../.gitbook/assets/image%20%2827%29.png)



