# BJS Security

BJS is very powerful and flexible. But with this. But simply make the code **vulnerable**.

{% hint style="danger" %}
Please read this article carefully

Especially if you work with payments, user balances, sell goods through a bot
{% endhint %}

## Any user can execute any command

Vulnerable command `/setBalance`:

```javascript
// admin can add 100$ to users's balance by it telegramid

tgID = params

let res = Libs.ResourcesLib.anotherUserRes("money", tgID);
res.add(100)
Bot.sendMessage("Added 100$ for user");
```

{% hint style="danger" %}
Any user can run /setBalance \[telegramid\]
{% endhint %}

So need check execute this command only for admin:

**1.Add this command to** [**group**](https://help.bots.business/commands/groups)

![](../.gitbook/assets/image%20%2834%29.png)

add admin to group "admin": 

```javascript
// create any temporary command with this code
// run it for admin

// destroy command after for security
// also you can protect this command with password

User.addToGroup("admin")
```

**2. Or you can check admin in BJS**

first you need get ADMIN\_TELEGRAM\_ID

```javascript
Bot.sendMessage(user.telegramid)
```

security command:

```javascript
if(user.telegramid!=ADMIN_TELEGRAM_ID){
  return // exit from BJS
}

// ONLY admin can add 100$ to users's balance by it telegramid

tgID = params

let res = Libs.ResourcesLib.anotherUserRes("money", tgID);
res.add(100)
Bot.sendMessage("Added 100$ for user");
```



## Any user can execute any "SECRET" command

For example, you have command `/payment` \(have "Wait for answer"\) with execute other "secret" command `/setBalance` :

```javascript
// command /payment

// user provide oneTime password. If password is valid - add bonus 100$

var oneTimePassword = User.getProperty("oneTimePassword");

if(!oneTimePassword){
  return // we have not oneTime password now
}

if(oneTimePassword=="already taked"){
  // if taked already - exit
  return
}

if(oneTimePassword!=message){
  // user do not know oneTime password
  Bot.sendMessager("Error. Password is wrong")
}

if(oneTimePassword==message){
  // user know oneTime password!
  // make it "already taked"
  User.setProperty("oneTimePassword", "already taked", "string")
  // run "secret" command
  Bot.runCommand("/setBalance");
  Bot.sendMessage("Thank you for payment!");
}
```

"Secret" command `/setBalance`

```javascript
let res = Libs.ResourcesLib.userRes("money", tgID);
res.add(100)
Bot.sendMessage("Added 100$ for you");
```

**So user must:**

* run /payment command
* type secret one time password
* after it - "secret" command "/setBalance" will be runned

**Vulnerability: hacker can run /setBalance only and get bonus immediately**

Need to checking that command `/setBalance` was runned only by command `/payment`

one of the methods - pass secret on run command as params:

command `/payment`

```javascript
...
// part of code for /payment

if(oneTimePassword==message){
  ...
  var secret = "GJHURFVJLHF" // use own secret. You can store it in property
  Bot.runCommand("/setBalance " + secret);
  Bot.sendMessage("Thank you for payment!");
}
```

{% hint style="danger" %}
Do not use "GJHURFVJLHF" secret! 

It is not secret world already: hacker can read this doc too!
{% endhint %}

command `/setBalance`

```javascript
if(params=="GJHURFVJLHF"){
   let res = Libs.ResourcesLib.userRes("money", tgID);
   res.add(100)
   Bot.sendMessage("Added 100$ for you");
}else{
   Bot.sendMessage("You are hacker!")
}
```



## User can run secret command on group chat

It can be accidentally or deliberately provoked by a hacker.

If you have a secret command with a secret result, do not run it in a group chat:

```javascript
// send this link only in PM - secure reason
if(chat.chat_type!="private"){
  return
}
```



## Recommendations

### Do not share your bot token, BB API Key

Bot token and BB API Key - are is very vulnerability data. Do not share theys anywhere!



### Do not share your BB Bot ID

This can sometimes be unsafe.



### Do not use default command names "/onIncome", "/onTransaction" for important commands

Hacker can brute force such command names and try to execute it



### **Remove /test command**

If you have any /test command with non security BJS - remove it.

Hacker can execute /test too



### Use `completed_commands_count` variable

Anybody can run any command. But it is possible make secured sub command.

For example command `/admin`

```javascript
// make admin access here
// ...

Bot.runCommand("/secure")
```

command `/secure`

```javascript
// this command can not be runned by user
if(completed_commands_count==0){ return }

// only via Bot.runCommand, Bot.run or as "on_result"
// your secure code here
// ...
```

## Do not use any non official libs now.

{% hint style="danger" %}
Do not use any non official libs now. 

* Any lib can run command with options.
* Any libs can read properties \(and read your API Keys from other lib\)

We have not way to protect this now. Just **not use NON official libs** with CP lib. Well, that now there are no such libraries
{% endhint %}

## Bad practice 

### User can change nickname

Bad BJS:

```javascript
let admin = "Jon Smith";

if (user.first_name==admin){
  // do admin action here
  ...
}
```

{% hint style="danger" %}
Any user can set any first\_name, last\_name and etc 

Hacker can change or create account with this field
{% endhint %}

### 

### Use eval method with care

You can use eval for calculation

```javascript
// with Wait for Answer

// message from user is: 2+2
// eval - it js execution from string
let result = eval(message);

// 2+2 = 4. So we have 4 in result now
Bot.sendMessage(result)
```

With such code you can make math calculator. 

But it is very danger! User can run anything!

For example, user can pass BJS code:  `bot.token`. And bot will send your bot token!

## See BB reports

[Read info](https://help.bots.business/bb-inspection) about BB report. Demo report have nice recommendations. 

