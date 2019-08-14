# Wait for answer

## What it is "Wait for answer"?

It is need the `Wait for answer` flag if need a response from the user.

![Can be modified on command editing](../.gitbook/assets/image%20%286%29.png)

Example of execution of one command:

Bot:

> What is your name?

User:

> Jon

Bot:

> Hello, Jon

command:

```javascript
answer: What is your name?
need_reply: true
BJS: Bot.sendMessage( "Hello, " + message );
```

So BJS code execute only after user's answer

## How to cancel "Wait for"?

Example of canceled for command with "Wait for":

Bot:

> What is your name?

User \(press "❌ Back" on keybord\):

> ❌ Back

BJS:

```javascript
if(message=="❌ Back"){
   return  // exit from command on "Back"
}
```

or you can run /menu command on "Back"

```javascript
if(message=="❌ Back"){
   Bot.runCommand("/menu")
   return // exit from command on "Back"
}
```



