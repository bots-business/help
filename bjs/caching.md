# Caching

Commands must be executed quickly within 100 - 250 ms. If commands take a long time to complete, users have a bad impression of using the bot

Some commands cannot be executed quickly:

* HTTP loading
* long api requests (for example, [sendVideo](https://core.telegram.org/bots/api#sendvideo), [getChatMember](https://core.telegram.org/bots/api#getchatmember)- all get methods and etc)
* mass message broadcasting
* Bot.runAll

Some commands can be very large or poorly coded:

* mass properting reading/setting (more then 20 properties in one command
* big (or infinite!) loops
* etc

**if your code runs slower than 300ms - necessary:**

* try to make it more quickly
* or cache it
* or run command on background and cache it (if needed)

{% hint style="danger" %}
If your code takes very long time it will be aborted with timeout error
{% endhint %}

{% hint style="danger" %}
Slow commands can cause your Cloud to **degrade and even crash.**

For example, let's say your cloud can process 1 request per second.

Thus, 4 requests of 250 ms each will be processed per second

But if you have slow commands (let's say one second), the cloud will process only one command instead of four.

Thus, the response to the first message will only be sent in a second. And the answer to the last is the fourth already in four.
{% endhint %}

## Caching methods

### setCache

Example set command caching for 1 hour (for all bot users):

```javascript
Bot.sendMessage("Hello!");

Bot.setCache(
  60*60 // time in seconds 60*60 = 1 hour
)
```

Example of command caching for 1 hour (for one user only):

```javascript
User.sendMessage("Hello, " + user.first_name);

User.setCache(
  60*60 // time in seconds 60*60 = 1 hour
)
```

### clearCache

you can clear Cache (it can be usefull on changing data)

```javascript
Bot.clearCache(
  "/command" // command Name
)
```

clear for user caching

```javascript
User.clearCache(
  "/command" // command Name
)
```

## What can be cached?

Caching is a powerful method for speeding up a bot. But you can't cache everything.

Criteria for caching:

* messages from the bot to this command do not change, or rarely change
* command accepts no params `(/command any param)` or options `(Bot.run(command: "/cmd", options: options))`, or accepts them, but they rarely change
* the result of the command is not critical. Even if the old, not updated value is returned, this is not critical

If your command does not meet these requirements try to devide it for several commands. One or more of them can be cached. To run them use methods: `Bot.run` or `Bot.runCommand`

## &#x20;Advanced techniques

You can PRE run caching in background for long command.

Command `/start`

```javascript
Bot.sendMessage("Welcome to my bot");

Bot.run({
  command: "/longBackroundTask",
  run_after: 1 // will be runned in background after 1 sec
})
```

Command `/longBackroundTask`

```javascript
// make your long task here...
Bot.getProperty("myJSON_1");
Bot.getProperty("myJSON_1");
// ... and etc...
Bot.getProperty("myJSON_100");

User.setCache(
  60*60*24*7 // time in seconds - 60*60*24*7 it is week
)
```

## Examples

command `/time`:&#x20;

```javascript
var date = new Date(); 

var time = "Time: " + 
   + date.getHours() + ":"  
   + date.getMinutes() + ":" 
   + date.getSeconds();

Bot.sendMessage(time);
```

We have such result (without caching) - take 250 ms:

![](<../.gitbook/assets/image (39).png>)

Edit command for caching:

```javascript
var date = new Date(); 

var time = "Time: " + 
   + date.getHours() + ":"  
   + date.getMinutes() + ":" 
   + date.getSeconds();

Bot.sendMessage(time);

Bot.setCache(120);  // caching for 120 seconds
```

Now we have such result - take 120 ms (instead 250 ms without caching):

![](<../.gitbook/assets/image (46).png>)

Command result is changed only after 120 seconds. During that 120 seconds answer is same.
